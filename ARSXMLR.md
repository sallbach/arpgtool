# ARSXMLR - XML-Tools

write a xml-file

    XmlDocument(CCSID_UTF8);
    XmlPrettyPrint(*on);
    
    XmlNode('root');
      XmlNode('child1');
        XmlNode('child11':'data 11');
          XmlAttribute('atr1':'value 1');
          XmlAttributeNum('atr2':1.23);
          XmlAttribute('atr3':'value 3');
        XmlNode('child12':'data 12');
      XmlNodeEnd();
      XmlNode('child2':'data 2&3');
      XmlNodeNum('child3':22.333);
      XmlNode('child4':'');
        XmlAttribute('atr1':'value "1"');
        XmlAttribute('atr2':'value 2');
        XmlAttributeBool('atr3':*on);
    XmlNodeEnd();
    XmlToStmf('/tmp/test.xml');

Result 

        <?xml version="1.0" encoding="UTF-8"?>
        <root>
          <child1>
            <child11 atr1="value 1" atr2="1.23" atr3="value 3">data 11</child11>
            <child12>data 12</child12>
          </child1>
          <child2>data 2&amp;3</child2>
          <child3>22.333</child3>
          <child4 atr1="value &quot;1&quot;" atr2="value 2" atr3="true" />
        </root>


