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

##Using path to dig into records and attributes

```xml

<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="Input">
    <Output>
      <OutputId>
        <!--The id of the input doc-->
        <xsl:value-of select="id"/>
      </OutputId>
      <OutputName>
        <xsl:value-of select="name"/>
        </OutputName>
      <AddressType>
        <xsl:value-of select="address/@type"/>
      </AddressType>
      <Street>
        <xsl:value-of select="address/street"/>
      </Street>
    </Output>
  </xsl:template>
</xsl:stylesheet>


```


##For each


```xml

<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="Input">
    <Output>
      <OutputId>
        <!--The id of the input doc-->
        <xsl:value-of select="id"/>
      </OutputId>
      <OutputName>
        <xsl:value-of select="name"/>
      </OutputName>

      <Addresses>
        <xsl:for-each select="address">
          <Address>
            <AddressType>
              <xsl:value-of select="@type"/>
            </AddressType>
            <Street>
              <xsl:value-of select="street"/>
            </Street>
          </Address>
        </xsl:for-each>

      </Addresses>

    </Output>
  </xsl:template>
</xsl:stylesheet>

```

sample xml:

```xml

<Input>
  <id>40</id>
  <name>Morten</name>
  <address type="private">
    <street>GoStreet</street>
    <number>50</number>
  </address>
  <address type="work">
    <street>GogStreet</street>
    <number>500</number>
  </address>
</Input>

```

## XSLT with namespaces

```xml

<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:source="sourcenamespace" 
                xmlns:ns0="oresund"
                exclude-result-prefixes="source">
  <xsl:template match="source:Input">
    <ns0:Output>
      <OutputId>
        <!--The id of the input doc-->
        <xsl:value-of select="source:id"/>
      </OutputId>
      <OutputName>
        <xsl:value-of select="source:name"/>
      </OutputName>

      <Addresses>
        <xsl:for-each select="source:address">
          <Address>
            <AddressType>
              <xsl:value-of select="@type"/>
            </AddressType>
            <Street>
              <xsl:value-of select="source:street"/>
            </Street>
          </Address>
        </xsl:for-each>

      </Addresses>

    </ns0:Output>
  </xsl:template>
</xsl:stylesheet>

```

sample:

```xml

<Input xmlns="sourcenamespace" xmlns:stupid="stupid">
  <id>40</id>
  <name>Morten</name>
  <address type="private">
    <street>GoStreet</street>
    <number>50</number>
  </address>
  <address type="work">
    <street>GogStreet</street>
    <number>500</number>
  </address>
</Input>

```


## Re-ordering


```xml

<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
 <!-- Make key array thing!!-->
  <xsl:key name="records-by-artist" match="Record" use="Artist" />
  <xsl:template match="Records">
    <Artists>
      <!--Resembles DISTINCT only going to give you each artist once!!!-->
      <xsl:for-each select="Record[count(. | key('records-by-artist',Artist)[1]) = 1]">
        <xsl:sort select="Artist"/>
        <Artist>
          <Name>
            <xsl:value-of select="Artist"/>
          </Name>
          <Albums>
            <!--Each Record for the given Artist-->
            <xsl:for-each select="key('records-by-artist',Artist)">
              <Album>
                <Title>
                  <xsl:value-of select="Title"/>
                </Title>
              </Album>
            </xsl:for-each>
          </Albums>
        </Artist>
      </xsl:for-each>

    </Artists>
  </xsl:template>
</xsl:stylesheet>

```

sample xml:

```xml

<Records>
  <Record>
    <Title>Please Please Me</Title>
    <Artist>Beatles</Artist>
  </Record>
  <Record>
    <Title>Help</Title>
    <Artist>Beatles</Artist>
  </Record>
  <Record>
    <Title>Purple Rain</Title>
    <Artist>Prince</Artist>
  </Record>
  <Record>
    <Title>Rubber Soul</Title>
    <Artist>Beatles</Artist>
  </Record>
  <Record>
    <Title>The Wall</Title>
    <Artist>Pink Floyd</Artist>
  </Record>
  <Record>
    <Title>Back In Black</Title>
    <Artist>AC/DC</Artist>
  </Record>
  <Record>
    <Title>Wish You were here</Title>
    <Artist>Pink Floyd</Artist>
  </Record>
</Records>


```


