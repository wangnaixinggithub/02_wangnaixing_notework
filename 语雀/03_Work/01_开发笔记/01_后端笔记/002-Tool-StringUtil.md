# tool-StringUtil

```java

public final class StringUtil {
    static Logger log = LoggerFactory.getLogger(StringUtil.class);
    private static final char[] DIGITS_LOWER = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
    private static final char[] DIGITS_UPPER = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'};
    public static final String EMPTY_STRING = "";
    private static final WordTokenizer CAMEL_CASE_TOKENIZER = new WordTokenizer() {
        protected void startSentence(StringBuffer buffer, char ch) {
            buffer.append(Character.toLowerCase(ch));
        }

        protected void startWord(StringBuffer buffer, char ch) {
            if (!this.isDelimiter(buffer.charAt(buffer.length() - 1))) {
                buffer.append(Character.toUpperCase(ch));
            } else {
                buffer.append(Character.toLowerCase(ch));
            }

        }

        protected void inWord(StringBuffer buffer, char ch) {
            buffer.append(Character.toLowerCase(ch));
        }

        protected void startDigitSentence(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void startDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDelimiter(StringBuffer buffer, char ch) {
            if (ch != '_') {
                buffer.append(ch);
            }

        }
    };
    private static final WordTokenizer PASCAL_CASE_TOKENIZER = new WordTokenizer() {
        protected void startSentence(StringBuffer buffer, char ch) {
            buffer.append(Character.toUpperCase(ch));
        }

        protected void startWord(StringBuffer buffer, char ch) {
            buffer.append(Character.toUpperCase(ch));
        }

        protected void inWord(StringBuffer buffer, char ch) {
            buffer.append(Character.toLowerCase(ch));
        }

        protected void startDigitSentence(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void startDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDelimiter(StringBuffer buffer, char ch) {
            if (ch != '_') {
                buffer.append(ch);
            }

        }
    };
    private static final WordTokenizer UPPER_CASE_WITH_UNDERSCORES_TOKENIZER = new WordTokenizer() {
        protected void startSentence(StringBuffer buffer, char ch) {
            buffer.append(Character.toUpperCase(ch));
        }

        protected void startWord(StringBuffer buffer, char ch) {
            if (!this.isDelimiter(buffer.charAt(buffer.length() - 1))) {
                buffer.append('_');
            }

            buffer.append(Character.toUpperCase(ch));
        }

        protected void inWord(StringBuffer buffer, char ch) {
            buffer.append(Character.toUpperCase(ch));
        }

        protected void startDigitSentence(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void startDigitWord(StringBuffer buffer, char ch) {
            if (!this.isDelimiter(buffer.charAt(buffer.length() - 1))) {
                buffer.append('_');
            }

            buffer.append(ch);
        }

        protected void inDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDelimiter(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }
    };
    private static final WordTokenizer LOWER_CASE_WITH_UNDERSCORES_TOKENIZER = new WordTokenizer() {
        protected void startSentence(StringBuffer buffer, char ch) {
            buffer.append(Character.toLowerCase(ch));
        }

        protected void startWord(StringBuffer buffer, char ch) {
            if (!this.isDelimiter(buffer.charAt(buffer.length() - 1))) {
                buffer.append('_');
            }

            buffer.append(Character.toLowerCase(ch));
        }

        protected void inWord(StringBuffer buffer, char ch) {
            buffer.append(Character.toLowerCase(ch));
        }

        protected void startDigitSentence(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void startDigitWord(StringBuffer buffer, char ch) {
            if (!this.isDelimiter(buffer.charAt(buffer.length() - 1))) {
                buffer.append('_');
            }

            buffer.append(ch);
        }

        protected void inDigitWord(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }

        protected void inDelimiter(StringBuffer buffer, char ch) {
            buffer.append(ch);
        }
    };

    public StringUtil() {
    }

    public static boolean isNullOrEmpty(String s) {
        return s == null || isBlank(s);
    }

    public static boolean equal(String s1, String s2, boolean p_ignoreCase) {
        if (s1 != null && s2 != null) {
            return p_ignoreCase ? s1.equalsIgnoreCase(s2) : s1.equals(s2);
        } else {
            return false;
        }
    }

    public static boolean isEmpty(String str) {
        return str == null || str.length() == 0;
    }

    public static boolean isNotEmpty(String str) {
        return str != null && str.length() > 0;
    }

    public static boolean isBlank(String str) {
        int length;
        if (str != null && (length = str.length()) != 0) {
            for(int i = 0; i < length; ++i) {
                if (!Character.isWhitespace(str.charAt(i))) {
                    return false;
                }
            }

            return true;
        } else {
            return true;
        }
    }

    public static boolean isNotBlank(String str) {
        int length;
        if (str != null && (length = str.length()) != 0) {
            for(int i = 0; i < length; ++i) {
                if (!Character.isWhitespace(str.charAt(i))) {
                    return true;
                }
            }

            return false;
        } else {
            return false;
        }
    }

    public static String defaultIfNull(String str) {
        return str == null ? "" : str;
    }

    public static String defaultIfNull(String str, String defaultStr) {
        return str == null ? defaultStr : str;
    }

    public static String defaultIfEmpty(String str) {
        return str == null ? "" : str;
    }

    public static String defaultIfEmpty(String str, String defaultStr) {
        return str != null && str.length() != 0 ? str : defaultStr;
    }

    public static String defaultIfBlank(String str) {
        return isBlank(str) ? "" : str;
    }

    public static String defaultIfBlank(String str, String defaultStr) {
        return isBlank(str) ? defaultStr : str;
    }

    public static String trimAll(String str) {
        if (isNullOrEmpty(str)) {
            return "";
        } else {
            str = trim(str);
            StringBuffer buf = new StringBuffer();

            for(int i = 0; i < str.length(); ++i) {
                String ch = substring(str, i, i + 1);
                if (isNotBlank(ch)) {
                    buf.append(ch);
                }
            }

            return buf.toString();
        }
    }

    public static String trim(String str) {
        return trim(str, (String)null, 0);
    }

    public static String trim(String str, String stripChars) {
        return trim(str, stripChars, 0);
    }

    public static String trimStart(String str) {
        return trim(str, (String)null, -1);
    }

    public static String trimStart(String str, String stripChars) {
        return trim(str, stripChars, -1);
    }

    public static String trimEnd(String str) {
        return trim(str, (String)null, 1);
    }

    public static String trimEnd(String str, String stripChars) {
        return trim(str, stripChars, 1);
    }

    public static String trimToNull(String str) {
        return trimToNull(str, (String)null);
    }

    public static String trimToNull(String str, String stripChars) {
        String result = trim(str, stripChars);
        return result != null && result.length() != 0 ? result : null;
    }

    public static String trimToEmpty(String str) {
        return trimToEmpty(str, (String)null);
    }

    public static String trimToEmpty(String str, String stripChars) {
        String result = trim(str, stripChars);
        return result == null ? "" : result;
    }

    private static String trim(String str, String stripChars, int mode) {
        if (str == null) {
            return null;
        } else {
            int length = str.length();
            int start = 0;
            int end = length;
            if (mode <= 0) {
                if (stripChars == null) {
                    while(start < end && Character.isWhitespace(str.charAt(start))) {
                        ++start;
                    }
                } else {
                    if (stripChars.length() == 0) {
                        return str;
                    }

                    while(start < end && stripChars.indexOf(str.charAt(start)) != -1) {
                        ++start;
                    }
                }
            }

            if (mode >= 0) {
                if (stripChars == null) {
                    while(start < end && Character.isWhitespace(str.charAt(end - 1))) {
                        --end;
                    }
                } else {
                    if (stripChars.length() == 0) {
                        return str;
                    }

                    while(start < end && stripChars.indexOf(str.charAt(end - 1)) != -1) {
                        --end;
                    }
                }
            }

            return start <= 0 && end >= length ? str : str.substring(start, end);
        }
    }

    public static boolean equals(String str1, String str2) {
        if (str1 == null) {
            return str2 == null;
        } else {
            return str1.equals(str2);
        }
    }

    public static boolean equalsIgnoreCase(String str1, String str2) {
        if (str1 == null) {
            return str2 == null;
        } else {
            return str1.equalsIgnoreCase(str2);
        }
    }

    public static boolean isAlpha(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isLetter(str.charAt(i))) {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isAlphaSpace(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isLetter(str.charAt(i)) && str.charAt(i) != ' ') {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isAlphanumeric(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isLetterOrDigit(str.charAt(i))) {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isAlphanumericSpace(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isLetterOrDigit(str.charAt(i)) && str.charAt(i) != ' ') {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isNumeric(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isDigit(str.charAt(i))) {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isNumericSpace(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isDigit(str.charAt(i)) && str.charAt(i) != ' ') {
                    return false;
                }
            }

            return true;
        }
    }

    public static boolean isWhitespace(String str) {
        if (str == null) {
            return false;
        } else {
            int length = str.length();

            for(int i = 0; i < length; ++i) {
                if (!Character.isWhitespace(str.charAt(i))) {
                    return false;
                }
            }

            return true;
        }
    }

    public static String toUpperCase(String str) {
        return str == null ? null : str.toUpperCase(Locale.ENGLISH);
    }

    public static String toLowerCase(String str) {
        return str == null ? null : str.toLowerCase();
    }

    public static String capitalize(String str) {
        int strLen;
        return str != null && (strLen = str.length()) != 0 ? (new StringBuffer(strLen)).append(Character.toTitleCase(str.charAt(0))).append(str.substring(1)).toString() : str;
    }

    public static String uncapitalize(String str) {
        int strLen;
        return str != null && (strLen = str.length()) != 0 ? (new StringBuffer(strLen)).append(Character.toLowerCase(str.charAt(0))).append(str.substring(1)).toString() : str;
    }

    public static String swapCase(String str) {
        int strLen;
        if (str != null && (strLen = str.length()) != 0) {
            StringBuffer buffer = new StringBuffer(strLen);
            char ch = false;

            for(int i = 0; i < strLen; ++i) {
                char ch = str.charAt(i);
                if (Character.isUpperCase(ch)) {
                    ch = Character.toLowerCase(ch);
                } else if (Character.isTitleCase(ch)) {
                    ch = Character.toLowerCase(ch);
                } else if (Character.isLowerCase(ch)) {
                    ch = Character.toUpperCase(ch);
                }

                buffer.append(ch);
            }

            return buffer.toString();
        } else {
            return str;
        }
    }

    public static String toCamelCase(String str) {
        return CAMEL_CASE_TOKENIZER.parse(str);
    }

    public static String toPascalCase(String str) {
        return PASCAL_CASE_TOKENIZER.parse(str);
    }

    public static String toUpperCaseWithUnderscores(String str) {
        return UPPER_CASE_WITH_UNDERSCORES_TOKENIZER.parse(str);
    }

    public static String toUpperCaseFirst(String str) {
        return isNullOrEmpty(str) ? "" : str.replaceFirst(str.substring(0, 1), str.substring(0, 1).toUpperCase(Locale.ENGLISH));
    }

    public static String toLowerCaseWithUnderscores(String str) {
        return LOWER_CASE_WITH_UNDERSCORES_TOKENIZER.parse(str);
    }

    public static String[] split(String str) {
        return split(str, (String)null, -1);
    }

    public static String[] split(String str, char separatorChar) {
        if (str == null) {
            return null;
        } else {
            int length = str.length();
            if (length == 0) {
                return new String[0];
            } else {
                List<String> list = new ArrayList();
                int i = 0;
                int start = 0;
                boolean match = false;

                while(i < length) {
                    if (str.charAt(i) == separatorChar) {
                        if (match) {
                            list.add(str.substring(start, i));
                            match = false;
                        }

                        ++i;
                        start = i;
                    } else {
                        match = true;
                        ++i;
                    }
                }

                if (match) {
                    list.add(str.substring(start, i));
                }

                return (String[])((String[])list.toArray(new String[list.size()]));
            }
        }
    }

    public static String[] split(String str, String separatorChars) {
        return split(str, separatorChars, -1);
    }

    public static String[] split(String str, String separatorChars, int max) {
        if (str == null) {
            return null;
        } else {
            int length = str.length();
            if (length == 0) {
                return new String[0];
            } else {
                List<String> list = new ArrayList();
                int sizePlus1 = 1;
                int i = 0;
                int start = 0;
                boolean match = false;
                if (separatorChars == null) {
                    while(i < length) {
                        if (Character.isWhitespace(str.charAt(i))) {
                            if (match) {
                                if (sizePlus1++ == max) {
                                    i = length;
                                }

                                list.add(str.substring(start, i));
                                match = false;
                            }

                            ++i;
                            start = i;
                        } else {
                            match = true;
                            ++i;
                        }
                    }
                } else if (separatorChars.length() == 1) {
                    char sep = separatorChars.charAt(0);

                    while(i < length) {
                        if (str.charAt(i) == sep) {
                            if (match) {
                                if (sizePlus1++ == max) {
                                    i = length;
                                }

                                list.add(str.substring(start, i));
                                match = false;
                            }

                            ++i;
                            start = i;
                        } else {
                            match = true;
                            ++i;
                        }
                    }
                } else {
                    while(i < length) {
                        if (separatorChars.indexOf(str.charAt(i)) >= 0) {
                            if (match) {
                                if (sizePlus1++ == max) {
                                    i = length;
                                }

                                list.add(str.substring(start, i));
                                match = false;
                            }

                            ++i;
                            start = i;
                        } else {
                            match = true;
                            ++i;
                        }
                    }
                }

                if (match) {
                    list.add(str.substring(start, i));
                }

                return (String[])((String[])list.toArray(new String[list.size()]));
            }
        }
    }

    public static String join(Object[] array) {
        return join((Object[])array, (String)null);
    }

    public static String join(Object[] array, char separator) {
        if (array == null) {
            return null;
        } else {
            int arraySize = array.length;
            int bufSize = arraySize == 0 ? 0 : ((array[0] == null ? 16 : array[0].toString().length()) + 1) * arraySize;
            StringBuffer buf = new StringBuffer(bufSize);

            for(int i = 0; i < arraySize; ++i) {
                if (i > 0) {
                    buf.append(separator);
                }

                if (array[i] != null) {
                    buf.append(array[i]);
                }
            }

            return buf.toString();
        }
    }

    public static String join(Object[] array, String separator) {
        if (array == null) {
            return null;
        } else {
            if (separator == null) {
                separator = "";
            }

            int arraySize = array.length;
            int bufSize = arraySize == 0 ? 0 : arraySize * ((array[0] == null ? 16 : array[0].toString().length()) + (separator != null ? separator.length() : 0));
            StringBuffer buf = new StringBuffer(bufSize);

            for(int i = 0; i < arraySize; ++i) {
                if (separator != null && i > 0) {
                    buf.append(separator);
                }

                if (array[i] != null) {
                    buf.append(array[i]);
                }
            }

            return buf.toString();
        }
    }

    public static String joinWithoutSpace(String[] array, String sep) {
        if (array != null && array.length != 0) {
            if (isNullOrEmpty(sep)) {
                sep = "";
            }

            StringBuffer buf = new StringBuffer(256);

            for(int i = 0; i < array.length; ++i) {
                if (!isNullOrEmpty(array[i])) {
                    if (i == 0) {
                        buf.append(array[i]);
                    } else {
                        buf.append(sep).append(array[i]);
                    }
                }
            }

            return buf.toString();
        } else {
            return null;
        }
    }

    public static String join(Iterator<?> iterator, char separator) {
        if (iterator == null) {
            return null;
        } else {
            StringBuffer buf = new StringBuffer(256);

            while(iterator.hasNext()) {
                Object obj = iterator.next();
                if (obj != null) {
                    buf.append(obj);
                }

                if (iterator.hasNext()) {
                    buf.append(separator);
                }
            }

            return buf.toString();
        }
    }

    public static String join(Iterator<?> iterator, String separator) {
        if (iterator == null) {
            return null;
        } else {
            StringBuffer buf = new StringBuffer(256);

            while(iterator.hasNext()) {
                Object obj = iterator.next();
                if (obj != null) {
                    buf.append(obj);
                }

                if (separator != null && iterator.hasNext()) {
                    buf.append(separator);
                }
            }

            return buf.toString();
        }
    }

    public static int indexOf(String str, char searchChar) {
        return str != null && str.length() != 0 ? str.indexOf(searchChar) : -1;
    }

    public static int indexOf(String str, char searchChar, int startPos) {
        return str != null && str.length() != 0 ? str.indexOf(searchChar, startPos) : -1;
    }

    public static int indexOf(String str, String searchStr) {
        return str != null && searchStr != null ? str.indexOf(searchStr) : -1;
    }

    public static int indexOf(String str, String searchStr, int startPos) {
        if (str != null && searchStr != null) {
            return searchStr.length() == 0 && startPos >= str.length() ? str.length() : str.indexOf(searchStr, startPos);
        } else {
            return -1;
        }
    }

    public static int indexOfAny(String str, char[] searchChars) {
        if (str != null && str.length() != 0 && searchChars != null && searchChars.length != 0) {
            for(int i = 0; i < str.length(); ++i) {
                char ch = str.charAt(i);

                for(int j = 0; j < searchChars.length; ++j) {
                    if (searchChars[j] == ch) {
                        return i;
                    }
                }
            }

            return -1;
        } else {
            return -1;
        }
    }

    public static int indexOfAny(String str, String searchChars) {
        if (str != null && str.length() != 0 && searchChars != null && searchChars.length() != 0) {
            for(int i = 0; i < str.length(); ++i) {
                char ch = str.charAt(i);

                for(int j = 0; j < searchChars.length(); ++j) {
                    if (searchChars.charAt(j) == ch) {
                        return i;
                    }
                }
            }

            return -1;
        } else {
            return -1;
        }
    }

    public static int indexOfAny(String str, String[] searchStrs) {
        if (str != null && searchStrs != null) {
            int sz = searchStrs.length;
            int ret = Integer.MAX_VALUE;
            int tmp = false;

            for(int i = 0; i < sz; ++i) {
                String search = searchStrs[i];
                if (search != null) {
                    int tmp = str.indexOf(search);
                    if (tmp != -1 && tmp < ret) {
                        ret = tmp;
                    }
                }
            }

            return ret == Integer.MAX_VALUE ? -1 : ret;
        } else {
            return -1;
        }
    }

    public static int indexOfAnyBut(String str, char[] searchChars) {
        if (str != null && str.length() != 0 && searchChars != null && searchChars.length != 0) {
            label29:
            for(int i = 0; i < str.length(); ++i) {
                char ch = str.charAt(i);

                for(int j = 0; j < searchChars.length; ++j) {
                    if (searchChars[j] == ch) {
                        continue label29;
                    }
                }

                return i;
            }

            return -1;
        } else {
            return -1;
        }
    }

    public static int indexOfAnyBut(String str, String searchChars) {
        if (str != null && str.length() != 0 && searchChars != null && searchChars.length() != 0) {
            for(int i = 0; i < str.length(); ++i) {
                if (searchChars.indexOf(str.charAt(i)) < 0) {
                    return i;
                }
            }

            return -1;
        } else {
            return -1;
        }
    }

    public static int lastIndexOf(String str, char searchChar) {
        return str != null && str.length() != 0 ? str.lastIndexOf(searchChar) : -1;
    }

    public static int lastIndexOf(String str, char searchChar, int startPos) {
        return str != null && str.length() != 0 ? str.lastIndexOf(searchChar, startPos) : -1;
    }

    public static int lastIndexOf(String str, String searchStr) {
        return str != null && searchStr != null ? str.lastIndexOf(searchStr) : -1;
    }

    public static int lastIndexOf(String str, String searchStr, int startPos) {
        return str != null && searchStr != null ? str.lastIndexOf(searchStr, startPos) : -1;
    }

    public static int lastIndexOfAny(String str, String[] searchStrs) {
        if (str != null && searchStrs != null) {
            int searchStrsLength = searchStrs.length;
            int index = -1;
            int tmp = false;

            for(int i = 0; i < searchStrsLength; ++i) {
                String search = searchStrs[i];
                if (search != null) {
                    int tmp = str.lastIndexOf(search);
                    if (tmp > index) {
                        index = tmp;
                    }
                }
            }

            return index;
        } else {
            return -1;
        }
    }

    public static boolean contains(String str, char searchChar) {
        if (str != null && str.length() != 0) {
            return str.indexOf(searchChar) >= 0;
        } else {
            return false;
        }
    }

    public static boolean contains(String str, String searchStr) {
        if (str != null && searchStr != null) {
            return str.indexOf(searchStr) >= 0;
        } else {
            return false;
        }
    }

    public static boolean containsOnly(String str, char[] valid) {
        if (valid != null && str != null) {
            if (str.length() == 0) {
                return true;
            } else if (valid.length == 0) {
                return false;
            } else {
                return indexOfAnyBut(str, valid) == -1;
            }
        } else {
            return false;
        }
    }

    public static boolean containsOnly(String str, String valid) {
        return str != null && valid != null ? containsOnly(str, valid.toCharArray()) : false;
    }

    public static boolean containsNone(String str, char[] invalid) {
        if (str != null && invalid != null) {
            int strSize = str.length();
            int validSize = invalid.length;

            for(int i = 0; i < strSize; ++i) {
                char ch = str.charAt(i);

                for(int j = 0; j < validSize; ++j) {
                    if (invalid[j] == ch) {
                        return false;
                    }
                }
            }

            return true;
        } else {
            return true;
        }
    }

    public static boolean containsNone(String str, String invalidChars) {
        return str != null && invalidChars != null ? containsNone(str, invalidChars.toCharArray()) : true;
    }

    public static int countMatches(String str, String subStr) {
        if (str != null && str.length() != 0 && subStr != null && subStr.length() != 0) {
            int count = 0;

            for(int index = 0; (index = str.indexOf(subStr, index)) != -1; index += subStr.length()) {
                ++count;
            }

            return count;
        } else {
            return 0;
        }
    }

    public static String substring(String str, int start) {
        if (str == null) {
            return null;
        } else {
            if (start < 0) {
                start += str.length();
            }

            if (start < 0) {
                start = 0;
            }

            return start > str.length() ? "" : str.substring(start);
        }
    }

    public static String substring(String str, int start, int end) {
        if (str == null) {
            return null;
        } else {
            if (end < 0) {
                end += str.length();
            }

            if (start < 0) {
                start += str.length();
            }

            if (end > str.length()) {
                end = str.length();
            }

            if (start > end) {
                return "";
            } else {
                if (start < 0) {
                    start = 0;
                }

                if (end < 0) {
                    end = 0;
                }

                return str.substring(start, end);
            }
        }
    }

    public static String left(String str, int len) {
        if (str == null) {
            return null;
        } else if (len < 0) {
            return "";
        } else {
            return str.length() <= len ? str : str.substring(0, len);
        }
    }

    public static String right(String str, int len) {
        if (str == null) {
            return null;
        } else if (len < 0) {
            return "";
        } else {
            return str.length() <= len ? str : str.substring(str.length() - len);
        }
    }

    public static String mid(String str, int pos, int len) {
        if (str == null) {
            return null;
        } else if (len >= 0 && pos <= str.length()) {
            if (pos < 0) {
                pos = 0;
            }

            return str.length() <= pos + len ? str.substring(pos) : str.substring(pos, pos + len);
        } else {
            return "";
        }
    }

    public static String substringBefore(String str, String separator) {
        if (str != null && separator != null && str.length() != 0) {
            if (separator.length() == 0) {
                return "";
            } else {
                int pos = str.indexOf(separator);
                return pos == -1 ? str : str.substring(0, pos);
            }
        } else {
            return str;
        }
    }

    public static String substringAfter(String str, String separator) {
        if (str != null && str.length() != 0) {
            if (separator == null) {
                return "";
            } else {
                int pos = str.indexOf(separator);
                return pos == -1 ? "" : str.substring(pos + separator.length());
            }
        } else {
            return str;
        }
    }

    public static String substringBeforeLast(String str, String separator) {
        if (str != null && separator != null && str.length() != 0 && separator.length() != 0) {
            int pos = str.lastIndexOf(separator);
            return pos == -1 ? str : str.substring(0, pos);
        } else {
            return str;
        }
    }

    public static String substringAfterLast(String str, String separator) {
        if (str != null && str.length() != 0) {
            if (separator != null && separator.length() != 0) {
                int pos = str.lastIndexOf(separator);
                return pos != -1 && pos != str.length() - separator.length() ? str.substring(pos + separator.length()) : "";
            } else {
                return "";
            }
        } else {
            return str;
        }
    }

    public static String substringBetween(String str, String tag) {
        return substringBetween(str, tag, tag, 0);
    }

    public static String substringBetween(String str, String open, String close) {
        return substringBetween(str, open, close, 0);
    }

    public static String substringBetween(String str, String open, String close, int fromIndex) {
        if (str != null && open != null && close != null) {
            int start = str.indexOf(open, fromIndex);
            if (start != -1) {
                int end = str.indexOf(close, start + open.length());
                if (end != -1) {
                    return str.substring(start + open.length(), end);
                }
            }

            return null;
        } else {
            return null;
        }
    }

    public static String deleteWhitespace(String str) {
        if (str == null) {
            return null;
        } else {
            int sz = str.length();
            StringBuffer buffer = new StringBuffer(sz);

            for(int i = 0; i < sz; ++i) {
                if (!Character.isWhitespace(str.charAt(i))) {
                    buffer.append(str.charAt(i));
                }
            }

            return buffer.toString();
        }
    }

    public static String replaceOnce(String text, String repl, String with) {
        return replace(text, repl, with, 1);
    }

    public static String replace(String text, String repl, String with) {
        return replace(text, repl, with, -1);
    }

    public static String replace(String text, String repl, String with, int max) {
        if (text != null && repl != null && with != null && repl.length() != 0 && max != 0) {
            StringBuffer buf = new StringBuffer(text.length());
            int start = 0;
            int end = false;

            int end;
            while((end = text.indexOf(repl, start)) != -1) {
                buf.append(text.substring(start, end)).append(with);
                start = end + repl.length();
                --max;
                if (max == 0) {
                    break;
                }
            }

            buf.append(text.substring(start));
            return buf.toString();
        } else {
            return text;
        }
    }

    public static String replaceChars(String str, char searchChar, char replaceChar) {
        return str == null ? null : str.replace(searchChar, replaceChar);
    }

    public static String replaceChars(String str, String searchChars, String replaceChars) {
        if (str != null && str.length() != 0 && searchChars != null && searchChars.length() != 0) {
            char[] chars = str.toCharArray();
            int len = chars.length;
            boolean modified = false;
            int i = 0;

            for(int isize = searchChars.length(); i < isize; ++i) {
                char searchChar = searchChars.charAt(i);
                int j;
                if (replaceChars != null && i < replaceChars.length()) {
                    for(j = 0; j < len; ++j) {
                        if (chars[j] == searchChar) {
                            chars[j] = replaceChars.charAt(i);
                            modified = true;
                        }
                    }
                } else {
                    j = 0;

                    for(int j = 0; j < len; ++j) {
                        if (chars[j] != searchChar) {
                            chars[j++] = chars[j];
                        } else {
                            modified = true;
                        }
                    }

                    len = j;
                }
            }

            if (!modified) {
                return str;
            } else {
                return new String(chars, 0, len);
            }
        } else {
            return str;
        }
    }

    public static String overlay(String str, String overlay, int start, int end) {
        if (str == null) {
            return null;
        } else {
            if (overlay == null) {
                overlay = "";
            }

            int len = str.length();
            if (start < 0) {
                start = 0;
            }

            if (start > len) {
                start = len;
            }

            if (end < 0) {
                end = 0;
            }

            if (end > len) {
                end = len;
            }

            if (start > end) {
                int temp = start;
                start = end;
                end = temp;
            }

            return (new StringBuffer(len + start - end + overlay.length() + 1)).append(str.substring(0, start)).append(overlay).append(str.substring(end)).toString();
        }
    }

    public static String chomp(String str) {
        if (str != null && str.length() != 0) {
            if (str.length() == 1) {
                char ch = str.charAt(0);
                return ch != '\r' && ch != '\n' ? str : "";
            } else {
                int lastIdx = str.length() - 1;
                char last = str.charAt(lastIdx);
                if (last == '\n') {
                    if (str.charAt(lastIdx - 1) == '\r') {
                        --lastIdx;
                    }
                } else if (last != '\r') {
                    ++lastIdx;
                }

                return str.substring(0, lastIdx);
            }
        } else {
            return str;
        }
    }

    public static String chomp(String str, String separator) {
        if (str != null && str.length() != 0 && separator != null) {
            return str.endsWith(separator) ? str.substring(0, str.length() - separator.length()) : str;
        } else {
            return str;
        }
    }

    public static String chop(String str) {
        if (str == null) {
            return null;
        } else {
            int strLen = str.length();
            if (strLen < 2) {
                return "";
            } else {
                int lastIdx = strLen - 1;
                String ret = str.substring(0, lastIdx);
                char last = str.charAt(lastIdx);
                return last == '\n' && ret.charAt(lastIdx - 1) == '\r' ? ret.substring(0, lastIdx - 1) : ret;
            }
        }
    }

    public static String repeat(String str, int repeat) {
        if (str == null) {
            return null;
        } else if (repeat <= 0) {
            return "";
        } else {
            int inputLength = str.length();
            if (repeat != 1 && inputLength != 0) {
                int outputLength = inputLength * repeat;
                int ch0;
                switch (inputLength) {
                    case 1:
                        char ch = str.charAt(0);
                        char[] output1 = new char[outputLength];

                        for(ch0 = repeat - 1; ch0 >= 0; --ch0) {
                            output1[ch0] = ch;
                        }

                        return new String(output1);
                    case 2:
                        ch0 = str.charAt(0);
                        char ch1 = str.charAt(1);
                        char[] output2 = new char[outputLength];

                        for(int i = repeat * 2 - 2; i >= 0; --i) {
                            output2[i] = (char)ch0;
                            output2[i + 1] = ch1;
                            --i;
                        }

                        return new String(output2);
                    default:
                        StringBuffer buf = new StringBuffer(outputLength);

                        for(int i = 0; i < repeat; ++i) {
                            buf.append(str);
                        }

                        return buf.toString();
                }
            } else {
                return str;
            }
        }
    }

    public static String alignLeft(String str, int size) {
        return alignLeft(str, size, ' ');
    }

    public static String alignLeft(String str, int size, char padChar) {
        if (str == null) {
            return null;
        } else {
            int pads = size - str.length();
            return pads <= 0 ? str : alignLeft(str, size, String.valueOf(padChar));
        }
    }

    public static String alignLeft(String str, int size, String padStr) {
        if (str == null) {
            return null;
        } else {
            if (padStr == null || padStr.length() == 0) {
                padStr = " ";
            }

            int padLen = padStr.length();
            int strLen = str.length();
            int pads = size - strLen;
            if (pads <= 0) {
                return str;
            } else if (pads == padLen) {
                return str.concat(padStr);
            } else if (pads < padLen) {
                return str.concat(padStr.substring(0, pads));
            } else {
                char[] padding = new char[pads];
                char[] padChars = padStr.toCharArray();

                for(int i = 0; i < pads; ++i) {
                    padding[i] = padChars[i % padLen];
                }

                return str.concat(new String(padding));
            }
        }
    }

    public static String alignRight(String str, int size) {
        return alignRight(str, size, ' ');
    }

    public static String alignRight(String str, int size, char padChar) {
        if (str == null) {
            return null;
        } else {
            int pads = size - str.length();
            return pads <= 0 ? str : alignRight(str, size, String.valueOf(padChar));
        }
    }

    public static String alignRight(String str, int size, String padStr) {
        if (str == null) {
            return null;
        } else {
            if (padStr == null || padStr.length() == 0) {
                padStr = " ";
            }

            int padLen = padStr.length();
            int strLen = str.length();
            int pads = size - strLen;
            if (pads <= 0) {
                return str;
            } else if (pads == padLen) {
                return padStr.concat(str);
            } else if (pads < padLen) {
                return padStr.substring(0, pads).concat(str);
            } else {
                char[] padding = new char[pads];
                char[] padChars = padStr.toCharArray();

                for(int i = 0; i < pads; ++i) {
                    padding[i] = padChars[i % padLen];
                }

                return (new String(padding)).concat(str);
            }
        }
    }

    public static String center(String str, int size) {
        return center(str, size, ' ');
    }

    public static String center(String str, int size, char padChar) {
        if (str != null && size > 0) {
            int strLen = str.length();
            int pads = size - strLen;
            if (pads <= 0) {
                return str;
            } else {
                str = alignRight(str, strLen + pads / 2, padChar);
                str = alignLeft(str, size, padChar);
                return str;
            }
        } else {
            return str;
        }
    }

    public static String center(String str, int size, String padStr) {
        if (str != null && size > 0) {
            if (padStr == null || padStr.length() == 0) {
                padStr = " ";
            }

            int strLen = str.length();
            int pads = size - strLen;
            if (pads <= 0) {
                return str;
            } else {
                str = alignRight(str, strLen + pads / 2, padStr);
                str = alignLeft(str, size, padStr);
                return str;
            }
        } else {
            return str;
        }
    }

    public static String reverse(String str) {
        return str != null && str.length() != 0 ? (new StringBuffer(str)).reverse().toString() : str;
    }

    public static String abbreviate(String str, int maxWidth) {
        return abbreviate(str, 0, maxWidth);
    }

    public static String abbreviate(String str, int offset, int maxWidth) {
        if (str == null) {
            return null;
        } else {
            if (maxWidth < 4) {
                maxWidth = 4;
            }

            if (str.length() <= maxWidth) {
                return str;
            } else {
                if (offset > str.length()) {
                    offset = str.length();
                }

                if (str.length() - offset < maxWidth - 3) {
                    offset = str.length() - (maxWidth - 3);
                }

                if (offset <= 4) {
                    return str.substring(0, maxWidth - 3) + "...";
                } else {
                    if (maxWidth < 7) {
                        maxWidth = 7;
                    }

                    return offset + (maxWidth - 3) < str.length() ? "..." + abbreviate(str.substring(offset), maxWidth - 3) : "..." + str.substring(str.length() - (maxWidth - 3));
                }
            }
        }
    }

    public static String difference(String str1, String str2) {
        if (str1 == null) {
            return str2;
        } else if (str2 == null) {
            return str1;
        } else {
            int index = indexOfDifference(str1, str2);
            return index == -1 ? "" : str2.substring(index);
        }
    }

    public static int indexOfDifference(String str1, String str2) {
        if (str1 != null && str2 != null && !str1.equals(str2)) {
            int i;
            for(i = 0; i < str1.length() && i < str2.length() && str1.charAt(i) == str2.charAt(i); ++i) {
            }

            return i >= str2.length() && i >= str1.length() ? -1 : i;
        } else {
            return -1;
        }
    }

    public static int getLevenshteinDistance(String s, String t) {
        s = defaultIfNull(s);
        t = defaultIfNull(t);
        int n = s.length();
        int m = t.length();
        if (n == 0) {
            return m;
        } else if (m == 0) {
            return n;
        } else {
            int[][] d = new int[n + 1][m + 1];

            int i;
            for(i = 0; i <= n; d[i][0] = i++) {
            }

            int j;
            for(j = 0; j <= m; d[0][j] = j++) {
            }

            for(i = 1; i <= n; ++i) {
                char s_i = s.charAt(i - 1);

                for(j = 1; j <= m; ++j) {
                    char t_j = t.charAt(j - 1);
                    byte cost;
                    if (s_i == t_j) {
                        cost = 0;
                    } else {
                        cost = 1;
                    }

                    d[i][j] = min(d[i - 1][j] + 1, d[i][j - 1] + 1, d[i - 1][j - 1] + cost);
                }
            }

            return d[n][m];
        }
    }

    private static int min(int a, int b, int c) {
        if (b < a) {
            a = b;
        }

        if (c < a) {
            a = c;
        }

        return a;
    }

    public static byte[] getBytesUtf8(String string) {
        try {
            return string.getBytes("UTF-8");
        } catch (Exception var2) {
            log.warn(var2.getMessage());
            return new byte[0];
        }
    }

    public static byte[] getBytesIso8859_1(String string) {
        try {
            return string.getBytes("ISO-8859-1");
        } catch (UnsupportedEncodingException var2) {
            log.warn(var2.getMessage());
            return new byte[0];
        }
    }

    public static String newStringIso8859_1(byte[] bytes) throws UnsupportedEncodingException {
        return new String(bytes, "ISO-8859-1");
    }

    public static String newStringUtf8(byte[] bytes) {
        try {
            return new String(bytes, "UTF-8");
        } catch (UnsupportedEncodingException var2) {
            log.warn(var2.getMessage());
            return null;
        }
    }

    public static byte[] getBytesUnchecked(String string, String charsetName) {
        if (string == null) {
            return null;
        } else {
            try {
                return string.getBytes(charsetName);
            } catch (UnsupportedEncodingException var3) {
                throw new RuntimeException("不支持的字符集：" + charsetName, var3);
            }
        }
    }

    public static String newString(byte[] bytes, String charsetName) {
        if (bytes == null) {
            return null;
        } else {
            try {
                return new String(bytes, charsetName);
            } catch (UnsupportedEncodingException var3) {
                throw new RuntimeException("不支持的字符集：" + charsetName, var3);
            }
        }
    }

    public static boolean startsWithIgnoreCase(String str, String prefix) {
        if (str != null && prefix != null) {
            if (str.startsWith(prefix)) {
                return true;
            } else if (str.length() < prefix.length()) {
                return false;
            } else {
                String lcStr = str.substring(0, prefix.length()).toLowerCase(Locale.ENGLISH);
                String lcPrefix = prefix.toLowerCase(Locale.ENGLISH);
                return lcStr.equals(lcPrefix);
            }
        } else {
            return false;
        }
    }

    public static boolean hasText(String str) {
        if (!hasLength(str)) {
            return false;
        } else {
            int strLen = str.length();

            for(int i = 0; i < strLen; ++i) {
                if (!Character.isWhitespace(str.charAt(i))) {
                    return true;
                }
            }

            return false;
        }
    }

    public static boolean hasLength(String str) {
        return str != null && str.length() > 0;
    }

    public static String generateSqlInStr(String[] strs) {
        if (strs != null && strs.length != 0) {
            int i = 0;
            String re = "";
            String[] var3 = strs;
            int var4 = strs.length;

            for(int var5 = 0; var5 < var4; ++var5) {
                String s = var3[var5];
                if (isNotBlank(s)) {
                    if (i == 0) {
                        re = "'" + s;
                        ++i;
                    } else {
                        re = re + "','" + s;
                    }
                }
            }

            re = re + "'";
            return re;
        } else {
            return "";
        }
    }

    public static String generateSqlInStr(String str) {
        if (isBlank(str)) {
            return "";
        } else {
            String[] strs = split(str, ",");
            return generateSqlInStr(strs);
        }
    }

    public static String generateSqlInStr(List<String> strColl) {
        String str = "";
        Iterator var2 = strColl.iterator();

        while(var2.hasNext()) {
            String tmp = (String)var2.next();
            if (isNotBlank(tmp)) {
                if (isBlank(str)) {
                    str = "'" + tmp + "'";
                } else {
                    str = str + ",'" + tmp + "'";
                }
            }
        }

        return str;
    }

    protected static char[] encodeHex(byte[] data, char[] toDigits) {
        int l = data.length;
        char[] out = new char[l << 1];
        int i = 0;

        for(int j = 0; i < l; ++i) {
            out[j++] = toDigits[(240 & data[i]) >>> 4];
            out[j++] = toDigits[15 & data[i]];
        }

        return out;
    }

    public static char[] encodeHex(byte[] data) {
        return encodeHex(data, true);
    }

    public static char[] encodeHex(byte[] data, boolean toLowerCase) {
        return encodeHex(data, toLowerCase ? DIGITS_LOWER : DIGITS_UPPER);
    }

    public static String encodeHexString(byte[] data) {
        return new String(encodeHex(data));
    }

    public static final String toString(Object value) {
        return value == null ? "" : value.toString();
    }

    public static byte[] byteMerger(byte[]... values) {
        int length_byte = 0;

        for(int i = 0; i < values.length; ++i) {
            length_byte += values[i].length;
        }

        byte[] all_byte = new byte[length_byte];
        int countLength = 0;

        for(int i = 0; i < values.length; ++i) {
            byte[] b = values[i];
            System.arraycopy(b, 0, all_byte, countLength, b.length);
            countLength += b.length;
        }

        return all_byte;
    }

    public static byte charToByte(char c) {
        return (byte)"0123456789ABCDEF".indexOf(c);
    }

    public static byte[] hexStringToBytes(String hexString) {
        if (hexString != null && !hexString.equals("")) {
            hexString = hexString.toUpperCase();
            int length = hexString.length() / 2;
            char[] hexChars = hexString.toCharArray();
            byte[] d = new byte[length];

            for(int i = 0; i < length; ++i) {
                int pos = i * 2;
                d[i] = (byte)(charToByte(hexChars[pos]) << 4 | charToByte(hexChars[pos + 1]));
            }

            return d;
        } else {
            return null;
        }
    }

    public static int length(String value) {
        int valueLength = 0;
        String chinese = "[Α-￥]";

        for(int i = 0; i < value.length(); ++i) {
            String temp = value.substring(i, i + 1);
            if (temp.matches(chinese)) {
                valueLength += 2;
            } else {
                ++valueLength;
            }
        }

        return valueLength;
    }

    private abstract static class WordTokenizer {
        protected static final char UNDERSCORE = '_';

        private WordTokenizer() {
        }

        public String parse(String str) {
            if (StringUtil.isEmpty(str)) {
                return str;
            } else {
                int length = str.length();
                StringBuffer buffer = new StringBuffer(length);

                for(int index = 0; index < length; ++index) {
                    char ch = str.charAt(index);
                    if (!Character.isWhitespace(ch)) {
                        if (!Character.isUpperCase(ch)) {
                            if (Character.isLowerCase(ch)) {
                                index = this.parseLowerCaseWord(buffer, str, index);
                            } else if (Character.isDigit(ch)) {
                                index = this.parseDigitWord(buffer, str, index);
                            } else {
                                this.inDelimiter(buffer, ch);
                            }
                        } else {
                            int wordIndex;
                            for(wordIndex = index + 1; wordIndex < length; ++wordIndex) {
                                char wordChar = str.charAt(wordIndex);
                                if (!Character.isUpperCase(wordChar)) {
                                    if (Character.isLowerCase(wordChar)) {
                                        --wordIndex;
                                    }
                                    break;
                                }
                            }

                            if (wordIndex != length && wordIndex <= index) {
                                index = this.parseTitleCaseWord(buffer, str, index);
                            } else {
                                index = this.parseUpperCaseWord(buffer, str, index, wordIndex);
                            }
                        }
                    }
                }

                return buffer.toString();
            }
        }

        private int parseUpperCaseWord(StringBuffer buffer, String str, int index, int length) {
            char ch = str.charAt(index++);
            if (buffer.length() == 0) {
                this.startSentence(buffer, ch);
            } else {
                this.startWord(buffer, ch);
            }

            while(index < length) {
                ch = str.charAt(index);
                this.inWord(buffer, ch);
                ++index;
            }

            return index - 1;
        }

        private int parseLowerCaseWord(StringBuffer buffer, String str, int index) {
            char ch = str.charAt(index++);
            if (buffer.length() == 0) {
                this.startSentence(buffer, ch);
            } else {
                this.startWord(buffer, ch);
            }

            for(int length = str.length(); index < length; ++index) {
                ch = str.charAt(index);
                if (!Character.isLowerCase(ch)) {
                    break;
                }

                this.inWord(buffer, ch);
            }

            return index - 1;
        }

        private int parseTitleCaseWord(StringBuffer buffer, String str, int index) {
            char ch = str.charAt(index++);
            if (buffer.length() == 0) {
                this.startSentence(buffer, ch);
            } else {
                this.startWord(buffer, ch);
            }

            for(int length = str.length(); index < length; ++index) {
                ch = str.charAt(index);
                if (!Character.isLowerCase(ch)) {
                    break;
                }

                this.inWord(buffer, ch);
            }

            return index - 1;
        }

        private int parseDigitWord(StringBuffer buffer, String str, int index) {
            char ch = str.charAt(index++);
            if (buffer.length() == 0) {
                this.startDigitSentence(buffer, ch);
            } else {
                this.startDigitWord(buffer, ch);
            }

            for(int length = str.length(); index < length; ++index) {
                ch = str.charAt(index);
                if (!Character.isDigit(ch)) {
                    break;
                }

                this.inDigitWord(buffer, ch);
            }

            return index - 1;
        }

        protected boolean isDelimiter(char ch) {
            return !Character.isUpperCase(ch) && !Character.isLowerCase(ch) && !Character.isDigit(ch);
        }

        protected abstract void startSentence(StringBuffer var1, char var2);

        protected abstract void startWord(StringBuffer var1, char var2);

        protected abstract void inWord(StringBuffer var1, char var2);

        protected abstract void startDigitSentence(StringBuffer var1, char var2);

        protected abstract void startDigitWord(StringBuffer var1, char var2);

        protected abstract void inDigitWord(StringBuffer var1, char var2);

        protected abstract void inDelimiter(StringBuffer var1, char var2);
    }
}

```

​	
