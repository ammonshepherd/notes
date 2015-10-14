`ag -l betech | xargs sed -i '' -e 's/http:\/\/lab7.betech.virginia.edu:8080\/geonetwork\/xml\/schemas\/iso19139\/schema.xsd/http:\/\/www.isotc211.org\/2005\/gmd\/gmd.xsd/g'`

`ag -l gmd:MD_BrgeoservereGraphic | xargs sed -i '' -e 's/gmd:MD_BrgeoservereGraphic/gmd:MD_BrowseGraphic/'`


ag -l gmd:onLine | xargs sed -i '' -e 's/\(gmd:onLine>\)\([0-9]*\)\(<\/gmd:MD_DigitalTransferOptions\)/\1\3/'

