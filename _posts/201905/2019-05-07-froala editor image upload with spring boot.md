---
layout: post
title: 'Spring Boot : Froala Editor 이미지 업로드 처리'
keywords: 'java,spring boot,javascript'
categories: java
tags: java springboot
image: https://images.unsplash.com/photo-1455146164878-6c4be78e1433?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=2000&h=1200&fit=crop&ixid=eyJhcHBfaWQiOjF9
image-sm: https://images.unsplash.com/photo-1455146164878-6c4be78e1433?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=500&h=300&fit=crop&ixid=eyJhcHBfaWQiOjF9
---

Spring Boot 로 Froala Editor 이미지 업로드 처리를 하는 서버측 소스

## 클라이언트 HTML

```html
<div id="froala-editor"></div>

<script>
    $('div#froala-editor').froalaEditor({
        imageUploadURL: '/image', // 업로드 처리 end point
        imageUploadParam: 'file', // 파일 파라메터명
        imageUploadMethod: 'POST',
        imageAllowedTypes: ['jpeg', 'jpg', 'png'],
        imageMaxSize: 2 * 1024 * 1024 // 최대 이미지 사이즈 : 2메가
    });
</script>
```

## 서버쪽 처리

### 사용한 라이브러리 maven dependency

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.9</version>
</dependency>
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>27.1-jre</version>
</dependency>

<!-- annotation 으로 getter, setter 생성 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

### UploadResult.java

업로드 처리결과 저장용( lombok annotation 적용 )

```java
import lombok.Builder;
import lombok.Data;

@Data
@Builder
public class UploadResult {

    private String path;
    private String name;
    private Long size;
}
```

### StorageServcie.java

파일 관련 서비스

```java
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.IOException;
import java.io.OutputStream;
import java.nio.file.Files;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.UUID;

import org.apache.commons.io.FileUtils;
import org.apache.commons.io.FilenameUtils;
import org.apache.commons.io.IOUtils;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

/**
 * 파일 저장 서비스
 * @author 슷호브
 */
@Service
public class StorageServcie {

    @Value("${upload.path}")
    String uploadPath;  // application.yml 에 정의( 업로드 파일이 들어갈 path (ex)/tmp/upload )

    /**
     *
     * @param file 업로드할 파일
     * @return UploadResult {path=201905/e98ff4f7-93a3-4aeb-813b-12f20a03db96.JPG,
     *         name=샘플파일.jpg, size=126495}
     * @throws IOException
     */
    public UploadResult upload(MultipartFile file) throws IOException {
        // 년월 별로 업로드 디렉토리 관리
        String path = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyyMMdd"));

        // 서버에 저장될 유니크한 파일명
        String id = UUID.randomUUID().toString();
        String extension = FilenameUtils.getExtension(file.getOriginalFilename());

        if (StringUtils.isNotEmpty(extension)) {
            id = id + "." + extension;
        }

        File save = new File(String.format("%s%s%s%s%s", uploadPath, File.separator, path, File.separator, id));

        FileUtils.forceMkdirParent(save);
        file.transferTo(save);

        return UploadResult.builder()
                    .path(String.format("%s%s%s", path, File.separator, id))
                    .name(file.getOriginalFilename())
                    .size(file.getSize())
                    .build();
    }

    /**
     * 파일 bytes 가져오기
     */
    public byte[] bytes(String path, String id) throws IOException {
        return Files.readAllBytes(
                new File(String.format(
                            "%s%s%s%s%s",
                            uploadPath, File.separator,
                            path, File.separator, id)).toPath()
                    );
    }

    /**
     * out 으로 파일 bytes 보내기
     */
    public void to(String path, String id, OutputStream out) throws IOException {
        IOUtils.copy(new ByteArrayInputStream(bytes(path, id)), out);
    }
}
```

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

### FileController.java

```java
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.google.common.collect.ImmutableMap;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.multipart.MultipartFile;

import lombok.extern.slf4j.Slf4j;

/**
 * 파일관련 컨트롤러
 *
 * @author 슷호브
 */
@Slf4j
@Controller
public class FileController {
    @Autowired
    private StorageServcie servcie;

    /**
     * Froala Editor 이미지 업로드
     */
    @PostMapping("/image")
    @ResponseBody
    public Map<String, String> imageUpload(HttpServletRequest request, @RequestParam("file") MultipartFile file) {

        try {
            UploadResult result = servcie.upload(file);

            log.debug("upload result : {}", result);

            // {"link" : "/image/201905/e98ff4f7-93a3-4aeb-813b-12f20a03db96.jpg"}
            return ImmutableMap.of(
                        "link",
                        String.format("%s/image/%s", request.getContextPath(), result.getPath()));

        } catch (Exception ex) {
            ex.printStackTrace();

            return ImmutableMap.of("error", ex.getMessage());
        }
    }

    /**
     * 이미지 다운로드 (ex) /image/201905/e98ff4f7-93a3-4aeb-813b-12f20a03db96.jpg
     */
    @GetMapping("/image/{path}/{id}")
    public void image(HttpServletResponse response, @PathVariable String path, @PathVariable String id) {
        try {
            servcie.to(path, id, response.getOutputStream());
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }
}
```

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
