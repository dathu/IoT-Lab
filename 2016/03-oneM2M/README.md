```java
public class ADN{
	private Container createAE(String cse, String rn){
        AE ae = new AE();
		URI uri = null;
		try {
			uri = new URI(cse);
		} catch (URISyntaxException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		CoapClient client = new CoapClient(uri);
		Request req = Request.newPost();
		req.getOptions().addOption(new Option(267, 2));
		req.getOptions().addOption(new Option(256, "admin:admin"));
		req.getOptions().setContentFormat(MediaTypeRegistry.APPLICATION_JSON);
		req.getOptions().setAccept(MediaTypeRegistry.APPLICATION_JSON);
		JSONObject obj = new JSONObject();
		obj.put("api","TempApp-ID");
		obj.put("rr","true");
		obj.put("rn", rn);
		JSONObject root = new JSONObject();
		root.put("m2m:ae", obj);
		String body = root.toString();
		System.out.println(body);
		req.setPayload(body);
		CoapResponse responseBody = client.advanced(req);
		String response = new String(responseBody.getPayload());
		System.out.println(response);
		JSONObject resp = new JSONObject(response);
		JSONObject container = (JSONObject) resp.get("m2m:ae");
		ae.setRn((String) container.get("rn"));
		ae.setTy((Integer) container.get("ty"));
		ae.setRi((String) container.get("ri"));
		ae.setPi((String) container.get("pi"));
		ae.setCt((String) container.get("ct"));
		ae.setLt((String) container.get("lt"));
		
		return ae;

	}
	
	private Container createContainer(String cse, String rn){
		Container container = new Container();

		URI uri = null;
		try {
			uri = new URI(cse);
		} catch (URISyntaxException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		CoapClient client = new CoapClient(uri);
		Request req = Request.newPost();
		req.getOptions().addOption(new Option(267, 3));
		req.getOptions().addOption(new Option(256, "admin:admin"));
		req.getOptions().setContentFormat(MediaTypeRegistry.APPLICATION_JSON);
		req.getOptions().setAccept(MediaTypeRegistry.APPLICATION_JSON);
		JSONObject obj = new JSONObject();
		obj.put("rn", rn);
		JSONObject root = new JSONObject();
		root.put("m2m:cnt", obj);
		String body = root.toString();
		System.out.println(body);
		req.setPayload(body);
		CoapResponse responseBody = client.advanced(req);
		
		String response = new String(responseBody.getPayload());
		System.out.println(response);
		JSONObject resp = new JSONObject(response);
		JSONObject cont = (JSONObject) resp.get("m2m:cnt");
		container.setRn((String) cont.get("rn"));
		container.setTy((Integer) cont.get("ty"));
		container.setRi((String) cont.get("ri"));
		container.setPi((String) cont.get("pi"));
		container.setCt((String) cont.get("ct"));
		container.setLt((String) cont.get("lt"));
		container.setSt((Integer) cont.get("st"));
		container.setOl((String) cont.get("ol"));
		container.setLa((String) cont.get("la"));
		
		return container;
	}
	private void createContentInstance(String cse){
		URI uri = null;
		try {
			uri = new URI(cse);
		} catch (URISyntaxException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		CoapClient client = new CoapClient(uri);
		Request req = Request.newPost();
		req.getOptions().addOption(new Option(267, 4));
		req.getOptions().addOption(new Option(256, "admin:admin"));
		req.getOptions().setContentFormat(MediaTypeRegistry.APPLICATION_JSON);
		req.getOptions().setAccept(MediaTypeRegistry.APPLICATION_JSON);
		JSONObject content = new JSONObject();
		content.put("cnf","message");
		JSONObject con = new JSONObject();
		content.put("con","12");
		JSONObject root = new JSONObject();
		root.put("m2m:cin", content);
		String body = root.toString();
		System.out.println(body);
		req.setPayload(body);
		CoapResponse responseBody = client.advanced(req);
		
		String response = new String(responseBody.getPayload());
		System.out.println(response);
			
	}
	public static void main(String args[]) {
		ADN adn = new ADN();
		AE ae = adn.createAE("coap://127.0.0.1:5683/~/Tanganelli-in-cse", "TempApp");
		Container container = adn.createContainer("coap://127.0.0.1:5683/~/Tanganelli-in-cse/Tanganelli-in-name/TempApp", "DATA");
		adn.createContentInstance("coap://127.0.0.1:5683/~/Tanganelli-in-cse/Tanganelli-in-name/TempApp/DATA");	    
	}
}
```