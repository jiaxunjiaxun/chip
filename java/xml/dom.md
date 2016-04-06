# Document Object Model

DOMParserService.java

    package mirror.android.Services;
    import java.io.InputStream;
    import javax.xml.parsers.DocumentBuilder;
    import javax.xml.parsers.DocumentBuilderFactory;
    import org.w3c.dom.Document;
    import org.w3c.dom.Element;
    import org.w3c.dom.Node;
    import org.w3c.dom.NodeList;
    import android.util.Log;

    /**
     * DOM解析器在解析XML文档时，会把文档中的所有元素，按照其出现的层次关系，解析成一个个Node对象(节点)。
     * Node对象提供了一系列常量来代表结点的类型当开发人员获得某个Node类型后，就可以把Node节点转换成相应节点对象(Node的子类对象)，
     * 以便于调用其特有的方法。Node对象提供了相应的方法去获得它的父结点或子结点。编程人员通过这些方法就可以读取整个XML文档的内容、
     * 或添加、修改、删除XML文档的内容
     * 缺点：一次性的完全加载整个xml文件，需要消耗大量的内存。
     */
    public class DOMParseService implements InterfaceXMLParseService{
    
        @Override
        public void getPersonsByParseXML(InputStream is) {
        
            try {
                DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
                DocumentBuilder db = dbf.newDocumentBuilder();
                Document doc = db.parse(is);
                doc.getDocumentElement().normalize();
                
                // 得到根元素,这里是persons
                NodeList personNodes = doc.getElementsByTagName("person");
                
                for (int i = 0; i < personNodes.getLength(); i++) {
                    Element element_person = (Element)personNodes.item(i);
                    Node node_id = element_person.getElementsByTagName("id").item(0);
                    Node node_name = element_person.getElementsByTagName("name").item(0);
                    Node node_age = element_person.getElementsByTagName("age").item(0);
                    // ...
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }