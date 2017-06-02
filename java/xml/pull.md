# XML Pull Parsing

~~~ java
package mirror.android.Services;

import java.io.InputStream;
import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserFactory;

/**
 * 优点：不是一次加载，中途可以停止。
 */
public class PULLParseService implements InterfaceXMLParseService {

    @Override
    public void getPersonsByParseXML(InputStream is) {
        String str = "";
        try {
            XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
            XmlPullParser parser = factory.newPullParser();
            parser.setInput(is,"utf-8");
            // parser的几种事件类型
            int type = parser.getEventType();
            while (type != XmlPullParser.END_DOCUMENT) {
                String typeName = parser.getName();
                switch (type) {
                    case XmlPullParser.START_DOCUMENT:
                        break;
                    case XmlPullParser.START_TAG:
                        if ("person".equals(typeName)) {
                        } else if ("id".equals(typeName)) {
                            str += "Id: " + parser.nextText();
                        } else if ("name".equals(typeName)) {
                            str += ", name " + parser.nextText();
                        } else if ("age".equals(typeName)) {
                            str += ", age " + parser.nextText();
                            str = "";
                        }
                        break;
                    case XmlPullParser.END_TAG:
                        break;
                    default:
                        break;
                }
                // Get next parsing event.最最重要的一步，pull解析中的特有的方法，解析下一个标签
                type = parser.next();
            }
            /**
             * for (int type = parser.getEventType(); type != XmlPullParser.END_DOCUMENT; type = parser.next()) { ... }
             */
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
~~~