---
layout: post
title: 'JAVA : itextpdf 로 기존 pdf 파일에 워터마크 및 이것저것 추가해 보기'
keywords: 'java'
categories: java
image: https://images.unsplash.com/photo-1555885215-cbd53c552665?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
image-sm: https://images.unsplash.com/photo-1555885215-cbd53c552665?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&h=1200&ixid=eyJhcHBfaWQiOjF9&ixlib=rb-1.2.1&q=80&w=2000
---

이미 만들어져 있는 pdf 파일을 읽어서 워터마크등 요것조것 추가한 다음에 다른 pdf 파일로 출력하는 예제임

## maven dependency

```xml
<dependency>
    <groupId>com.itextpdf</groupId>
    <artifactId>itextpdf</artifactId>
    <version>5.5.13.1</version>
</dependency>

<!-- 굳이 필요는 없지만 파일관련 처리를 쉽게 할려고 가져다 씀 -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
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

## 샘플 소스 & 설명

```java
package io.github.stove99.sample;

import java.io.File;
import java.io.IOException;

import com.itextpdf.text.BaseColor;
import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.Font;
import com.itextpdf.text.Image;
import com.itextpdf.text.PageSize;
import com.itextpdf.text.Phrase;
import com.itextpdf.text.pdf.BaseFont;
import com.itextpdf.text.pdf.PdfContentByte;
import com.itextpdf.text.pdf.PdfGState;
import com.itextpdf.text.pdf.PdfImportedPage;
import com.itextpdf.text.pdf.PdfPCell;
import com.itextpdf.text.pdf.PdfPTable;
import com.itextpdf.text.pdf.PdfReader;
import com.itextpdf.text.pdf.PdfWriter;

import org.apache.commons.io.FileUtils;

public class PDFTest {

    public static void main(String[] args) throws IOException, DocumentException {
        Document document = new Document(PageSize.A4);

        // 출력할 pdf 파일
        PdfWriter writer = PdfWriter.getInstance(document, FileUtils.openOutputStream(new File("2018_out.pdf")));

        document.open();

        PdfContentByte canvas = writer.getDirectContent();

        // 원본 pdf 파일(클래스패스에서 가져옴)
        PdfReader reader = new PdfReader(PDFTest.class.getResourceAsStream("/2018.pdf"));

        // 워터마크 이미지(클래스패스에서 가져옴)
        Image img = Image.getInstance(PDFTest.class.getClassLoader().getResource("do-not-copy.png"));
        img.setAbsolutePosition(200, 300);
        img.scalePercent(20);

        // 원본 pdf 에서 페이지를 하나씩 읽어서 출력 pdf 에 추가한다.
        for (int num = 1, size = reader.getNumberOfPages(); num <= size; num++) {
            PdfImportedPage page = writer.getImportedPage(reader, num);

            document.newPage();

            // 0, 0 위치에 페이지 추가(참고로 itext에서 좌표 체계는 왼쪽 아래에서 부터 0, 0으로 시작함, 그래프와 비슷함)
            canvas.addTemplate(page, 0, 0);

            // 첫번째 페이지만 상단에 테이블 추가
            if (num == 1) {
                // (0, -1) : 전체 row, 뒤에 숫자 두개는 추가할 위치(x, y) = 왼쪽에서 20, top+20 위치에 테이블 추가
                makeHeader("1234567", "홍길동").writeSelectedRows(0, -1, 20, document.top() + 20, canvas);
            }

            // 이미지 투명하게 처리
            PdfGState state = new PdfGState();
            state.setFillOpacity(0.2f);
            canvas.setGState(state);

            // 워터마트 이미지 추가
            canvas.addImage(img);
        }

        document.close();
    }

    private static PdfPTable makeHeader(String doc_no, String writer) throws DocumentException, IOException {
        // 클래스패스 루트에 나눔고딕 폰트 추가(한글을 쓰기 위해서 한글폰트를 설정해 줘야됨)
        BaseFont base_font = BaseFont.createFont("NanumGothic.ttf", BaseFont.IDENTITY_H, BaseFont.EMBEDDED);

        Font font = new Font(base_font, 10);
        Font header_font = new Font(base_font, 10);
        header_font.setColor(BaseColor.WHITE);

        // 컬럼 4개 짜리 테이블
        PdfPTable table = new PdfPTable(4);
        table.setTotalWidth(new float[] { 80, 80, 80, 80 });
        table.setLockedWidth(true);

        // 컬럼이 4개니까 addCell 4번 해주면 됨
        PdfPCell cell = new PdfPCell(new Phrase("문서번호", header_font));
        cell.setFixedHeight(18);
        cell.setBackgroundColor(BaseColor.GRAY);
        table.addCell(cell);

        cell = new PdfPCell(new Phrase(doc_no, font));
        table.addCell(cell);

        cell = new PdfPCell(new Phrase("작성자", header_font));
        cell.setBackgroundColor(BaseColor.GRAY);
        table.addCell(cell);

        cell = new PdfPCell(new Phrase(writer, font));
        table.addCell(cell);

        return table;
    }
}
```

## 결과물

<img src="/assets/attach/201907/pdf_result.png" style="width:900px;">

<ins class="adsbygoogle"
     style="display:block; text-align:center;"
     data-ad-layout="in-article"
     data-ad-format="fluid"
     data-ad-client="ca-pub-7073298118440059"
     data-ad-slot="8400970402"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>
