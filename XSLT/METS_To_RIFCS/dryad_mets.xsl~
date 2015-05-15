<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"	
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform" 
    xmlns:oai="http://www.openarchives.org/OAI/2.0/"  
    xmlns:mets="http://www.loc.gov/METS/"
    xmlns:mods="http://www.loc.gov/mods/v3" 
    xmlns:xlink="http://www.w3.org/1999/xlink" 
    xmlns="http://ands.org.au/standards/rif-cs/registryObjects"
    exclude-result-prefixes="xs xsi xsl oai mets mods xlink">

    <!-- =========================================== -->
    <!-- Configuration                               -->
    <!-- =========================================== -->

    <xsl:output method="xml" version="1.0" encoding="UTF-8" indent="yes"/>
    <xsl:param name="global_group" select="'Dryad'"/>
    <!-- <xsl:param name="global_acronym" select="'Dryad'"/> -->
    <xsl:param name="originatingSource" select="'Dryad'"/> 

    <!-- =========================================== -->
    <!-- RegistryObjects (root) Template             -->
    <!-- =========================================== -->
    
    <xsl:template match="/">
        <registryObjects>
            <xsl:attribute name="xsi:schemaLocation">
                <xsl:text>http://ands.org.au/standards/rif-cs/registryObjects http://services.ands.org.au/documentation/rifcs/schema/registryObjects.xsd</xsl:text>
            </xsl:attribute>
		
            <xsl:for-each select="//mets:mets">
		<xsl:if test=".//mods:genre='Dataset'">
           	   <xsl:apply-templates select="." mode="collection"/>
                </xsl:if> 
             </xsl:for-each>
        </registryObjects>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject Collection template          -->
    <!-- =========================================== -->
  
    <xsl:template match="mets:mets" mode="collection">
	<registryObject>
	    <!-- registryObject:@group -->
	    <xsl:attribute name="group">
		<xsl:value-of select="$global_group"/>
            </xsl:attribute>

	    <xsl:apply-templates select=".//mods:identifier[not(@type)]" mode="registryObject_key"/> 	

	    <originatingSource>
                <xsl:value-of select="$originatingSource"/>    
            </originatingSource>
	
            <collection>
		<xsl:attribute name="type">
		   <xsl:text>collection</xsl:text>
                </xsl:attribute>

		<xsl:apply-templates select=".//mods:identifier" mode="identifier"/> 	
		<xsl:apply-templates select=".//mods:identifier[@type='uri']" mode="url"/>
                <xsl:apply-templates select="." mode="dataset_desription"/> 
		<xsl:apply-templates select="." mode="dates"/>
                <xsl:apply-templates select="." mode="name"/> 
		<xsl:apply-templates select=".//mods:subject/mods:topic"/>
		<xsl:if test=".//mods:geographic | .//mods:temporal">
  		    <xsl:apply-templates select="." mode="coverage"/>
		</xsl:if>
		<xsl:apply-templates select=".//mods:relatedItem"/>
		<xsl:apply-templates select=".//mods:accessCondition"/>
            </collection>
	</registryObject>
    </xsl:template> 

 
    <!-- =========================================== -->
    <!-- RegistryObject/key template                 -->
    <!-- =========================================== -->

    <!-- <xsl:template match="mets:dmdSec" mode="registryObject_key">
        <key>
	    <xsl:variable name="identificator" select="@ID"/> 	
            <xsl:value-of select="concat($global_acronym,'/', $identificator)"/>
        </key>
    </xsl:template> -->

    <xsl:template match="mods:identifier" mode="registryObject_key">
        <key>
            <xsl:value-of select="normalize-space(.)"/>
        </key>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/identifier(doi) template     -->
    <!-- =========================================== -->

    <xsl:template match="mods:identifier" mode="identifier">
        <identifier>
	    <xsl:variable name="identifier" select="normalize-space(.)"/>	
	    <xsl:attribute name="type">
		<xsl:if test="contains($identifier, 'doi')">
		    <xsl:text>doi</xsl:text>
                </xsl:if>   
		<xsl:if test="contains($identifier, 'hdl.handle.net')">
		    <xsl:text>nandle</xsl:text>
                </xsl:if>   
            </xsl:attribute>
            <xsl:value-of select="normalize-space(.)"/>
        </identifier>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/location/address (url)       -->
    <!-- =========================================== -->

    <xsl:template match="mods:identifier" mode="url">	
	<location>
	    <address>
		<electronic>
		     <xsl:attribute name="type">
			<xsl:text>url</xsl:text>
            	     </xsl:attribute>
	
		     <xsl:value-of select="normalize-space(.)"/>
		</electronic>
	    </address>
	</location>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/description template         -->
    <!-- =========================================== -->

    <xsl:template match="mods:abstract">
	<description>
	    <xsl:attribute name="type">
		<xsl:text>full</xsl:text>
            </xsl:attribute>
	    <xsl:value-of select="."/>
	</description>
    </xsl:template> 

    <!-- =========================================== -->
    <!-- RegistryObject/description template         -->
    <!-- =========================================== -->

    <xsl:template match="mets:mets" mode="dataset_desription">
	<xsl:variable name="description" select=".//mods:note"/>
	<xsl:if test="$description | .//mods:role/mods:roleTerm='author'">
	    <description>
	        <xsl:attribute name="type">
		    <xsl:text>full</xsl:text>
                </xsl:attribute>
	
		<xsl:value-of select="$description"/>
		<xsl:if test=".//mods:role/mods:roleTerm='author'">
   	            <xsl:for-each select=".//mods:name">
			<xsl:if test="mods:role/mods:roleTerm='author'">
		            <xsl:text>&#xa;Author: </xsl:text>
			    <xsl:value-of select="mods:namePart"/>	
 		        </xsl:if>
	            </xsl:for-each>
		</xsl:if>
	     </description>
	</xsl:if>
    </xsl:template> 	

    <!-- =========================================== -->
    <!-- RegistryObject/dates template               -->
    <!-- =========================================== -->

    <xsl:template match="mets:mets" mode="dates">
	<dates>
	    <xsl:apply-templates select="mets:metsHdr"/>
	    <xsl:apply-templates select=".//mods:dateAvailable"/>
	    <xsl:apply-templates select=".//mods:dateIssued"/>
	</dates>
    </xsl:template> 	

    <!-- =========================================== -->
    <!-- RegistryObject/dates/date(dc.dateSubmitted) -->
    <!-- =========================================== -->

    <xsl:template match="mets:metsHdr">
        <date>
	    <xsl:attribute name="type">
		<xsl:text>dc.dateSubmitted</xsl:text>
            </xsl:attribute>
	    <xsl:value-of select="normalize-space(@CREATEDATE)"/>	
	</date>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/dates/date(dc.available)     -->
    <!-- =========================================== -->

    <xsl:template match="mods:dateAvailable">
        <date>
	    <xsl:attribute name="type">
		<xsl:text>dc.available</xsl:text>
            </xsl:attribute>
	    <xsl:value-of select="normalize-space(.)"/>	
	</date>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/dates/date(dc.issued)        -->
    <!-- =========================================== -->

    <xsl:template match="mods:dateIssued">
        <date>
	    <xsl:attribute name="type">
		<xsl:text>dc.issued</xsl:text>
            </xsl:attribute>
	    <xsl:value-of select="normalize-space(.)"/>	
	</date>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/name template               -->
    <!-- =========================================== -->

    <xsl:template match="mets:mets" mode="name">
	<name>
	    <xsl:apply-templates select=".//mods:titleInfo"/>
	</name>
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/name/namePart(primary) -->
    <!-- =========================================== -->

    <xsl:template match="mods:titleInfo">
	<namePart>
	    <xsl:attribute name="type">
		<xsl:text>primary</xsl:text>
            </xsl:attribute>
	    <xsl:value-of select="normalize-space(.)"/>
	</namePart>	
    </xsl:template>


    <!-- =========================================== -->
    <!-- RegistryObject/subject                     -->
    <!-- =========================================== -->

    <xsl:template match="mods:topic">
	<subject>
	    <xsl:attribute name="type">
		<xsl:text>local</xsl:text>
            </xsl:attribute>	
	    <xsl:value-of select="normalize-space(.)"/>
	</subject>	
    </xsl:template>	


    <!-- =========================================== -->
    <!-- RegistryObject/coverage template            -->
    <!-- =========================================== -->

    <xsl:template match="mets:mets" mode="coverage">
	<coverage>
	    <xsl:apply-templates select=".//mods:geographic"/>
	    <xsl:apply-templates select=".//mods:temporal"/>
	</coverage>
    </xsl:template>

   
    <!-- =========================================== -->
    <!-- RegistryObject/coverage/spatial             -->
    <!-- =========================================== -->

    <xsl:template match="mods:geographic">
	<spatial>
	    <xsl:value-of select="normalize-space(.)"/>
	</spatial>	
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/coverage/temporal/text       -->
    <!-- =========================================== -->

    <xsl:template match="mods:temporal">
	<temporal>
	    <text>
		<xsl:value-of select="normalize-space(.)"/>
	    </text>
	</temporal>	
    </xsl:template>

    <!-- =========================================== -->
    <!-- RegistryObject/relatedObject                -->
    <!-- =========================================== -->

    <xsl:template match="mods:relatedItem">
	<relatedObject>
	    <key>
		<xsl:value-of select="normalize-space(.)"/>
	    </key>
	    <xsl:variable name="type" select="@type"/>
	    <xsl:if test="@type='host'">
		<relation>
		   <xsl:attribute name="type">
			<xsl:text>isPartOf</xsl:text>
            	   </xsl:attribute> 	
		</relation>
	    </xsl:if>	
	</relatedObject>	
    </xsl:template>	

    <!-- =========================================== -->
    <!-- RegistryObject/rights                       -->
    <!-- =========================================== -->

    <xsl:template match="mods:accessCondition">
	<rights>
	    <licence>
		<xsl:attribute name="rightsUri">
		    <xsl:value-of select="normalize-space(.)"/>
            	</xsl:attribute> 
	    </licence>
	</rights>	
    </xsl:template>    	
</xsl:stylesheet>
