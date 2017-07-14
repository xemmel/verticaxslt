# Vertica's XSLT Tutorial

## A Basic XSLT

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:template match="Input">
    <Output>
        <ID>10</ID>
    </Output>
</xsl:template>
</xsl:stylesheet>

```

## With Namespaces

We're using **s** for _namespace-prefix_ for the source document and **t** for the target document.

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" 
xmlns:s="sourcenamespace" xmlns:t="targetnamespace">
<xsl:template match="s:Input">
    <t:Output>
        <ID>10</ID>
    </t:Output>
</xsl:template>
</xsl:stylesheet>

```


##Wellform XML in VS

CTRL+K + D


## Simple element mapping

```xml

<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="Input">
    <Output>
      <OutputId>
        <!--The id of the input doc-->
        <xsl:value-of select="id"/>
      </OutputId>
    </Output>
  </xsl:template>
</xsl:stylesheet>

```




