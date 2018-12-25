# ARSXMLR - XML-Tools

write a xml-file

    XmlDocument(CCSID_UTF8);
    XmlPrettyPrint(*on);
    XmlSetNullOnNull(*on); 
    XmlPi('xml-stylesheet type="text/xsl" href="test.xsl"');
    
    XmlNode('root');
      XmlNode('child1');
        XmlComment('Comment before child11');
        XmlNode('child11':'data 11'); // nodes with value must not be closed with XmlNodeEnd.
          XmlAttribute('atr1':'value 1');
          XmlAttributeNum('atr2':1.23);
          XmlAttribute('atr3':'value 3');
        XmlNode('child12':'data 12');
      XmlNodeEnd(); // end of "child1"
      XmlNode('child2':'data 2&3');
      XmlNodeNum('child3':22.333);
      XmlNode('child4':'');
        XmlAttribute('atr1':'value "1"');
        XmlAttribute('atr2':'value 2');
        XmlAttributeBool('atr3':*on); 
      // the following node will be ignored because the 3rd 
      // parameter is set *on and XmlSetNullOnNull is *on.    
      XmlNode('child_null':'ignored value':*on); 
      XmlNode('child_null_if_variable_blank':variable:(variable = *Blanks)); 
    XmlNodeEnd('root'); // end of "root". optional you can give the node name 
    XmlToStmf('/tmp/test.xml');

Result 

        <?xml version="1.0" encoding="UTF-8" ?>
        <?xml-stylesheet type="text/xsl" href="test.xsl" ?>
        <root>
          <!-- Comment before child11 -->
          <child1>
            <child11 atr1="value 1" atr2="1.23" atr3="value 3">data 11</child11>
            <child12>data 12</child12>
          </child1>
          <child2>data 2&amp;3</child2>
          <child3>22.333</child3>
          <child4 atr1="value &quot;1&quot;" atr2="value 2" atr3="true" />
        </root>

Please check the unit test module ARPG_TEST/T_ARSXMLR too.
