##在 pom.xml 添加依赖
```
 <!-- https://mvnrepository.com/artifact/com.itextpdf/itextpdf -->
        <dependency>
            <groupId>com.itextpdf</groupId>
            <artifactId>itextpdf</artifactId>
            <version>5.5.13</version>
        </dependency>

```
##具体代码为
```java
package pdf;

import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.pdf.AcroFields;
import com.itextpdf.text.pdf.PdfCopy;
import com.itextpdf.text.pdf.PdfImportedPage;
import com.itextpdf.text.pdf.PdfReader;
import com.itextpdf.text.pdf.PdfStamper;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author lixiaobao
 * @create 2019-07-22 10:22 AM
 **/

public class Test {
  public static void main(String[] args) throws Exception {
    String templatePath = "/Users/lixiaohuan/Downloads/123.pdf";
    //生成的新文件路径
    String newPDFPath = "/Users/lixiaohuan/Downloads/12341.pdf";
    PdfReader reader;
    FileOutputStream out;
    ByteArrayOutputStream bos;
    PdfStamper stamper;
    try {
      out = new FileOutputStream(newPDFPath);//输出流
      reader = new PdfReader(templatePath);//读取pdf模板
      bos = new ByteArrayOutputStream();
      stamper = new PdfStamper(reader, bos);
      AcroFields form = stamper.getAcroFields();

      java.util.Iterator<String> it = form.getFields().keySet().iterator();
      while(it.hasNext()){
        String name = it.next().toString();
        System.out.println(name + ":" + form.getFieldType(name));
        if(form.getFieldType(name) == AcroFields.FIELD_TYPE_TEXT) {
          form.setField(name, "130222111133338888");
        }else if(form.getFieldType(name) == AcroFields.FIELD_TYPE_CHECKBOX){
          form.setField(name,"On");
        }

      }
      stamper.setFormFlattening(false);//如果为false那么生成的PDF文件还能编辑，一定要设为true
      stamper.close();

      Document doc = new Document();
      PdfCopy copy = new PdfCopy(doc, out);
      doc.open();
      PdfImportedPage importPage = copy.getImportedPage(
              new PdfReader(bos.toByteArray()), 1);
      copy.addPage(importPage);
      doc.close();

    } catch (IOException e) {
      System.out.println(1);
    } catch (DocumentException e) {
      System.out.println(2);
    }
  }
}

```
