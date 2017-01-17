# Xml-to-Array-in-php


if ( !function_exists('xml2array') ) {
	function xml2array($xml){
    $opened = array();
    $xmlarray = array();
    $xml_parser = xml_parser_create();
    xml_parse_into_struct($xml_parser, $xml, $xmlarray);
    $array = array();
    $arrsize = sizeof($xmlarray);
    for($j=0;$j<$arrsize;$j++){
        $val = $xmlarray[$j];
        switch($val["type"]){
            case "open":
                $opened[$val["tag"]] = $array;
                unset($array);
                break;
            case "complete":
                $array[$val["tag"]][] = $val["value"];
            break;
            case "close":
                $opened[$val["tag"]] = $array;
                $array = $opened;
            break;
        }
    }
    return array_filter($array);
	}
}
