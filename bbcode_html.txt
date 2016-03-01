// bbCode a HTML

// Designar el estilo de una imagen
function Estilos(text){
    var result="";
    var cont;
    var estilo=new Array();
    estilo=text.split(",");
    for (cont=0;cont<estilo.length;cont++){
        switch (estilo[cont]){
            case "top":
                result+="vertical-align: "+estilo[cont]+"; "
                break;
            case "middle":
                result+="vertical-align: "+estilo[cont]+"; "
                break;
            case "bottom":
                result+="vertical-align: "+estilo[cont]+"; "
                break;
            case "center":
                result+="float: "+estilo[cont]+"; "
                break;  
            case "left":
                result+="float: "+estilo[cont]+"; "
                break;
            case "right":
                result+="float: "+estilo[cont]+"; "
                break;
            default:
                if (estilo[cont].substr(0,1)=="h"){
                    result+="height: "+estilo[cont].substr(1)+"px; ";
                    } else {
                        if (estilo[cont].substr(0,1)=="w") {
                            result+="width: "+estilo[cont].substr(1)+"px; ";
                            }
                        }
            }
        }
    return result
    }
    
// Copiar a Portapapeles
function clipboardCopy(txt) {
    if (window.clipboardData) {
        window.clipboardData.clearData();
        window.clipboardData.setData("Text", txt);
    }
    else if (window.netscape) {
        try {
            netscape.security.PrivilegeManager.enablePrivilege("UniversalXPConnect");
        }
        catch (e) {
            alert("Un script no puede Cortar / Copiar / Pegar automáticamente por razones de seguridad.\n"+
                  "Para hacerlo necesitas activar 'signed.applets.codebase_principal_support' en about:config'");
            return false;
        }
        var clip = Components.classes['@mozilla.org/widget/clipboard;1'].createInstance(Components.interfaces.nsIClipboard);
        if (!clip)
            return;
        var trans = Components.classes['@mozilla.org/widget/transferable;1'].createInstance(Components.interfaces.nsITransferable);
        if (!trans)
            return;
        trans.addDataFlavor('text/unicode');
        var str = new Object();
        var len = new Object();
        var str = Components.classes['@mozilla.org/supports-tring;1'].createInstance(Components.interfaces.nsISupportsString);
        var copytext = txt;
        str.data = copytext;
        trans.setTransferData("text/unicode",str,copytext.length*2);
        var clipid = Components.interfaces.nsIClipboard;
        if (!clip)
            return false;
        clip.setData(trans,null,clipid.kGlobalClipboard);
    }
}
// Fin de Copiar a Portapapeles

// Obtener URL
function bbcode2html_dameurl(destinourl,texturl,marcoparaurl){
		var result="";		
		var dominio="";
		var patron="^(http|https|ftp|file|news|data)://"
		
		destinourl=destinourl.replace(/(^\s*)|(\s*$)/g,""); //elimina espacios a izquierda y derecha
		var destinourlminusculas=destinourl.toLowerCase();
		if (destinourlminusculas.search(patron) != 0){
		   destinourl="http://"+destinourl;
		   }
		
		//var urlventanapadre="http://sites.google.com/site/fdvcreations/pruebas";		
		var urlventanapadre= _args()["parent"];	
		
		var posbarra=urlventanapadre.indexOf ("/",7);
		dominio=urlventanapadre.substr (0,posbarra);
				
		if (marcoparaurl==""){		  
		   patron= "^"+dominio;		   
		   if (destinourl.search(patron)==0){
		   	  marcoparaurl="_top";
		   	  } else {
			  marcoparaurl="_blank";
			  }
		   }		
		result+='<a href="';										   
	    result+=destinourl;
	    result+='" target="'+marcoparaurl+'">';
		result+=texturl;
		result+="</a>";
		return result
		}
		
// Conversión a HTML el text BBcode
function bbcode2html(cadena) {

         var carpetagrf_bbcode2html="http://bbcode2html-para-gadget.googlecode.com/files/";
		 
		 var result=""; var etiqueta="";var etiqueta_esperada=""; var contenido=""; var marco="";
		 var style=0; var letra="";
		 
		 for (var ct=0;ct<cadena.length;ct++) {
		 	 
			 letra=cadena.charAt(ct);	 
		 	 if (letra=="["&&style==0) {
			 	 etiqueta=""; style=1; }
				 
			 if (letra=="]"&&style==1) {
			     style=2;      	 	 }
				 
		 	 switch (style) {
			 		case 0: result+=letra; break
					case 1: etiqueta+=letra;  break
					case 2:
						 // evaluamos la etiqueta
						 etiqueta+=letra;
						 style=0;
						 if (etiqueta.lastIndexOf("=") ==-1) {
						 	switch (etiqueta) {
						 		case "[b]": result+="<b>"; break
								case "[/b]": result+="</b>"; break
								case "[i]": result+="<i>"; break
								case "[/i]": result+="</i>"; break
								case "[u]": result+="<u>"; break
								case "[/u]": result+="</u>"; break
								case "[hr]": result+="<hr>"; break
								case "[br]": result+="<br>"; break
								case "[p]": result+="<br clear=all>"; break
								case "[list]":result+="<ul>";break
								case "[/list]":result+="</ul>";break
								case "[*]":result+="<li>";break
								case "[quote]":result+="<blockquote>";break
								case "[/quote]":result+="</blockquote>";break
								
								case "[code]":style=3; contenido=""; etiqueta_esperada="[/code]"; break
								//case "[code]":result+="<code><pre>";break
								//case "[/code]":result+="</pre></code>";break
					            case "[block]": result+='<div style="display:inline;  float: left;">'; break
								case "[/block]": result+='</div>';break
								case "[left]":result+='<div style="text-align: left;">';break
								case "[/left]":result+='</div>';break
								case "[right]":result+='<div style="text-align: right;">';break
								case "[/right]":result+='</div>';break
								case "[center]":result+='<div style="text-align: center;">';break
								case "[/center]":result+='</div>';break
								case "[justify]":result+='<div style="text-align: justify;">';break
								case "[/justify]":result+='</div>';break
								
								case "[h1]":result+="<h1>";break
								case "[/h1]":result+="</h1>";break
								case "[h2]":result+="<h2>";break
								case "[/h2]":result+="</h2>";break
								case "[h3]":result+="<h3>";break
								case "[/h3]":result+="</h3>";break
								case "[h4]":result+="<h4>";break
								case "[/h4]":result+="</h4>";break
								case "[h5]":result+="<h5>";break
								case "[/h5]":result+="</h5>";break
								case "[h6]":result+="<h6>";break
								case "[/h6]":result+="</h6>";break
								
								case "[sup]":result+="<sup>";break
								case "[/sup]":result+="</sup>";break
								case "[sub]":result+="<sub>";break
								case "[/sub]":result+="</sub>";break
								
						        case "[img]": style=3; contenido=""; etiqueta_esperada="[/img]"; break
								
								case "[url]": style=3; contenido=""; etiqueta_esperada="[/url]"; break
								case "[url-in]": style=3; contenido=""; etiqueta_esperada="[/url]"; break
								case "[url-out]": style=3; contenido=""; etiqueta_esperada="[/url]"; break								
								case "[email]": style=3; contenido=""; etiqueta_esperada="[/email]"; break
								case "[copy]": style=3; contenido=""; etiqueta_esperada="[/copy]"; break
                                case "[clipboard]": style=3; contenido=""; etiqueta_esperada="[/clipboard]"; break								
								case "[/font]": result+="</span>"; break
								case "[/color]": result+="</span>"; break
								case "[/size]": result+="</span>"; break								
								case "[google]": style=3; contenido=""; etiqueta_esperada="[/google]"; break
								case "[wikipedia]": style=3; contenido=""; etiqueta_esperada="[/wikipedia]"; break
							
								default: result+=etiqueta;
						 		} // fin del switch etiqueta	
							} else {
							
							   style=4; contenido="";
							   etiqueta_esperada="[/"+etiqueta.substring(1,etiqueta.lastIndexOf("="))+"]";
							   
							   switch (etiqueta_esperada){
							   		  case "[/url-in]": etiqueta_esperada="[/url]";break
									  case "[/url-out]": etiqueta_esperada="[/url]";break
							   		  
									  case "[/font]":
									  	   style=0;
										   result+='<span style="font-family:';
										   result+=etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1);
										   result+='">';
										   break
									  case "[/size]":
									  	   style=0;
										   result+='<span style="font-size:';
										   result+=etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1);
										   result+='">';
										   break
									  case "[/color]":
									  	   style=0;
										   result+='<span style="color:';
										   result+=etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1);
										   result+='">';
										   break
									  case "[/list]":
									       style=0;
										   result+='<ul style="list-style-type: ';
										   switch (etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1)) {
										   		  case "c": result+='circle">';break
												  case "d": result+='disc">';break
												  case "s": result+='square">';break
												  case "1": result+='decimal">';break
												  case "a": result+='lower-alpha">';break
												  case "A": result+='upper-alpha">';break
												  case "i": result+='lower-roman">';break
												  case "I": result+='upper-roman">';break
												  default: result+='circle">';break
										   		  }
										   break
									  case "[/quote]":
									  	   style=0;
										   result+="<blockquote><i>";
										   result+=etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1);
										   result+=" escribi&oacute;:</i><br>";
										   break
							   		  }							   
							   }
						 	break 
					case 3:
						 contenido+=letra;
						 if (contenido.lastIndexOf("]") !=-1) {
						 	if (contenido.substring(contenido.lastIndexOf("["),contenido.lastIndexOf("]")+1)==etiqueta_esperada) { 	
							   switch (etiqueta_esperada) {
							   		  case "[/url]":
									  	   switch (etiqueta){
										   		  case "[url-in]": marco="_top";break
												  case "[url-out]": marco="_blank"; break
												  default: marco=""
												  }	                                                  			  
									  	   result+=bbcode2html_dameurl(contenido.substring(0,contenido.lastIndexOf("[")),contenido.substring(0,contenido.lastIndexOf("[")),marco);
									  	   style=0;
										   break
									  case "[/img]":
									  	   result+='<img src="';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='">';
										   
										   style=0;
										   break
								       
                                       case "[/clipboard]":
                                           result+='<span style="border:black solid 1px;     margin: 2px; padding: 4px; background-color:white; color: black;">';								  	   
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   //result+='&nbsp;<input type="button" value="copiar" onclick="clipboardCopy(';
                                     	   result+='&nbsp;<span onclick="clipboardCopy(';
                                           result+="'";
                                           result+=contenido.substring(0,contenido.lastIndexOf("["));								
                                           result+="'";
                                           result+=');" style="';   
                                           result+='text-decoration: none; ';
                                           result+='border-width:2px; ';
                                           result+='color: #000000; ';
                                           result+='font-weight: bold; ';
                                           result+='background-color: #CCCCCC; ';
                                           result+='border-style: outset; ';
                                           result+='cursor: default; ';                                           
                                           result+=' " ';                                           
                                           result+='onMouseOver="';
                                           result+="this.style.color='#FFFFFF';this.style.backgroundcolor='#999999'; this.style.borderStyle='outset';";
                                           result+='" onMouseOut="';
                                           result+="this.style.color='#000000';this.style.backgroundcolor='#000000'; this.style.borderStyle='outset';";
                                           result+='" onMouseDown="';
                                           result+="this.style.color='#FFFFFF';this.style.backgroundcolor='#666666'; this.style.borderStyle='inset';";
                                           result+='" onMouseUp="';
                                           result+="this.style.color='#FFFFFF';this.style.backgroundcolor='#999999'; this.style.borderStyle='outset';";    
										   //result+=');" />';
                                           result+='">copiar</span>';
										   result+='</span>';
                                           style=0;
										   break

                                       case "[/copy]":						  	   
                                            result+='<input type="text" name="text" value="';
                                            result+=contenido.substring(0,contenido.lastIndexOf("["));
                                            result+='" size=';
                                            result+=contenido.substring(0,contenido.lastIndexOf("[")).length+4;
                                            result+=' onClick="this.select()" readonly>';
		                                    style=0;
										   break							
                                		   
									   case "[/code]":
									  	   
										   result+='<code><pre>';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='</pre></code>';
										   style=0;
										   break
										   
									  case "[/email]":
									  	   result+='<a href="mailto: ';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='">';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+="</a>";
										   
										   style=0;
										   break
									  case "[/google]":
									  	   result+='<a href="http://www.google.com/search?q=';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='" target="_blank">';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='</a>';
										   style=0;
										   break
									  case "[/wikipedia]":
									  	   result+='<a href="http://es.wikipedia.org/wiki/';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='" target="_blank">';
										   result+=contenido.substring(0,contenido.lastIndexOf("["));
										   result+='</a>';
										   style=0;
										   break
									  }
							   }
							}
						break
					case 4:
						 contenido+=letra;
						 if (contenido.lastIndexOf("]") !=-1) {
						 	if (contenido.substring(contenido.lastIndexOf("["),contenido.lastIndexOf("]")+1)==etiqueta_esperada) { 	
							   
							   switch (etiqueta_esperada) {
							   		  case "[/url]":
									       switch (etiqueta.substring(1,etiqueta.lastIndexOf("="))){
										   		  case "url-in": marco="_top";break
												  case "url-out": marco="_blank"; break
												  default: marco=""
												  }
                                           result+=bbcode2html_dameurl(etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1),bbcode2html(contenido.substring(0,contenido.lastIndexOf("["))),marco);
                                           style=0;
										   break
							   		  case "[/img]":
									  	   result+='<img src="';
										   result+=bbcode2html(contenido.substring(0,contenido.lastIndexOf("[")));										   
										   result+='"';// align="';
										   //result+=etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1);										   
                                           result+=' style="';
                                           result+=Estilos(etiqueta.substring(etiqueta.lastIndexOf("=")+1,etiqueta.length-1));
										   result+='">';
										   style=0;
										   break									  
									  }
							   }
							}
						break					 
			 		} // fin del switch style
		 	 } // fin del for
		 
		 if (style>0) {result=result+etiqueta+contenido;}
		 
		 return result;
		 }
// Fin del convertidor BBcode a HTML
