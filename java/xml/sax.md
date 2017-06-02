# Simple API for XML

~~~ java
package mirror.android.Services;

import java.io.InputStream;
import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

/**
 * 优点：SAX解析允许在读取文档的时候，即对文档进行处理，而不必等到整个文档装载完才会文档进行操作。
 */
public class SAXParseService implements InterfaceXMLParseService {

    @Override
    public void getPersonsByParseXML(InputStream is) {
        try {
            SAXParserFactory factory = SAXParserFactory.newInstance();
            SAXParser parser = factory.newSAXParser();
            parser.parse(is, new saxHandler());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 自定义一个handler实现DefaultHandler并 重写它的相关方法
     */
    private class saxHandler extends DefaultHandler {

        private String currentTag = null;
        String str = "";

        @Override
        public void startDocument() throws SAXException {
            // 读到 <?xml version="1.0" encoding="utf-8"?> 时调用
        }

        @Override
        public void endDocument() throws SAXException {}

        @Override
        public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
            currentTag = localName;
        }

        @Override
        public void endElement(String uri, String localName, String qName) throws SAXException {
            if (localName.equals(currentTag)) {
                currentTag = null;
            }
        }

        @Override
        public void characters(char[] ch, int start, int length) throws SAXException {
            if ("id".equals(currentTag)) {
                str += "Id: " + new String(ch,start,length);
            }
            if ("name".equals(currentTag)) {
                str += ",Name: " + new String(ch,start,length);
            }
            if ("age".equals(currentTag)) {
                str += ",Age: " + new String(ch,start,length);
                str = "";
            }
        }
    }
}
~~~
