# tool-JsonUtil

```java
public class JsonUtil {
    static final ObjectMapper objectMapper = new ObjectMapper();
    static Logger log = LoggerFactory.getLogger(JsonUtil.class);
    private static final TypeReference<?> MAP_TYPE = new MapTypeReference();
    private static final TypeReference<?> LIST_TYPE = new ListTypeReference();

    public JsonUtil() {
    }

    public static String obj2json(Object obj) {
        try {
            return objectMapper.writeValueAsString(obj);
        } catch (JsonProcessingException var2) {
            log.warn(var2.getMessage());
            return "";
        }
    }

    public static <T> T json2Obj(String json, Class<T> clazz) {
        try {
            return objectMapper.readValue(json, clazz);
        } catch (Exception var3) {
            throw new RuntimeException("解析发生异常：" + var3.getMessage());
        }
    }

    public static List<Map<String, Object>> json2List(String json) {
        try {
            return (List)objectMapper.readValue(json, List.class);
        } catch (Exception var2) {
            throw new RuntimeException("解析发生异常：" + var2.getMessage());
        }
    }

    public static JsonNode getNode(String json, String nodeName) throws JsonProcessingException, IOException {
        JsonNode node = objectMapper.readTree(json);
        return node.get(nodeName);
    }

    public static JsonNode getNode(String json) {
        try {
            return objectMapper.readTree(json);
        } catch (Exception var2) {
            log.warn(var2.getMessage());
            throw new RuntimeException("解析json出错:" + var2.getMessage());
        }
    }

    public static JsonNode getNode(InputStream json, String nodeName) {
        try {
            JsonNode node = objectMapper.readTree(json);
            return node.get(nodeName);
        } catch (Exception var3) {
            throw new RuntimeException("解析出错：" + var3.getMessage());
        }
    }

    public static <T> T jsonNode2GenericObject(JsonNode node, TypeReference<T> tr) {
        try {
            return objectMapper.readValue(node.toString(), tr);
        } catch (Exception var3) {
            throw new RuntimeException("解析出错:" + var3.getMessage());
        }
    }

    public static <T> T json2GenericObject(String jsonString, TypeReference<T> tr) {
        try {
            return StringUtil.isNullOrEmpty(jsonString) ? null : objectMapper.readValue(jsonString, tr);
        } catch (Exception var3) {
            throw new RuntimeException("解析出错:" + var3.getMessage());
        }
    }

    public static ArrayNode newArrayNode() {
        return objectMapper.createArrayNode();
    }

    public static ArrayNode toArrayNode(String json) {
        try {
            return (ArrayNode)objectMapper.readValue(json, ArrayNode.class);
        } catch (Exception var2) {
            throw new RuntimeException("解析json错误");
        }
    }

    public static boolean isJsonObjectString(String str) {
        return str != null && str.matches("^\\{.*\\}$");
    }

    public static boolean isJsonArrayString(String str) {
        return str != null && str.matches("^\\[.*\\]$");
    }

 

    public static List<Object> parseListType(String json) {
        try {
            return (List)objectMapper.readValue(json, LIST_TYPE);
        } catch (Exception var2) {
            throw new IllegalArgumentException("不能解析 JSON", var2);
        }
    }

    public static Map<String, Object> parseMapType(String json) {
        try {
            return (Map)objectMapper.readValue(json, MAP_TYPE);
        } catch (Exception var2) {
            throw new IllegalArgumentException("不能解析 JSON", var2);
        }
    }

    static {
        objectMapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")).configure(Feature.ALLOW_UNQUOTED_FIELD_NAMES, true).configure(Feature.ALLOW_SINGLE_QUOTES, true).configure(SerializationFeature.INDENT_OUTPUT, true).configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false).configure(SerializationFeature.FAIL_ON_EMPTY_BEANS, false);
    }

    private static class ListTypeReference extends TypeReference<List<Object>> {
        private ListTypeReference() {
        }
    }

    private static class MapTypeReference extends TypeReference<Map<String, Object>> {
        private MapTypeReference() {
        }
    }
}

```

