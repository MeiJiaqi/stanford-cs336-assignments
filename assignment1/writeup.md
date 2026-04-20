### Problem (unicode1): Understanding Unicode (1 point)

**(a) What Unicode character does `chr(0)` return?**
*Deliverable: A one-sentence response.*

> **Your Answer:**
>
> **空字符串：**'\x00'，通常表示为null。

---

**(b) How does this character’s string representation (`__repr__()`) differ from its printed representation?**
*Deliverable: A one-sentence response.*

> **Your Answer:**
> 它的字符串表示`__repr__()`）会显式地显示为十六进制转义序列 `'\x00'`，而它的打印表示通常是完全不可见的，因为它是一个不可打印的控制字符。

---

**(c) What happens when this character occurs in text?**
*It may be helpful to play around with the following in your Python interpreter and see if it matches your expectations:*

```python
>>> chr(0)
>>> print(chr(0))
>>> "this is a test" + chr(0) + "string"
>>> print("this is a test" + chr(0) + "string")
```

*Deliverable: A one-sentence response.*

> **Your Answer:**  
> 该字符会被正常嵌入到字符串中，但由于它是不可打印的，所以在执行打印操作时它是不可见的，使得前后的文本看起来像是直接连在一起的。

### Problem (unicode2): Unicode Encodings (3 points)

**(a) What are some reasons to prefer training our tokenizer on UTF-8 encoded bytes, rather than UTF-16 or UTF-32? It may be helpful to compare the output of these encodings for various input strings.**

*Deliverable: A one-to-two sentence response.*

> **Your Answer:**
>
> UTF-8 是互联网上占据绝对主导地位的编码格式（超过 98% 的网页使用）且向后兼容 ASCII，这使得它对绝大多数常规英文文本更加节省内存（仅需 1 个字节，而 UTF-16 和 UTF-32 至少需要 2 个或 4 个字节）。这种高效性使得模型需要处理的输入序列更短，同时能维持 256 大小的初始基础词表。

---

**(b) Consider the following (incorrect) function, which is intended to decode a UTF-8 byte string into a Unicode string. Why is this function incorrect? Provide an example of an input byte string that yields incorrect results.**

```python

def decode_utf8_bytes_to_str_wrong(bytestring: bytes):

    return "".join([bytes([b]).decode("utf-8") for b in bytestring])

>>> decode_utf8_bytes_to_str_wrong("hello".encode("utf-8"))

'hello'
```

> **Your Answer:**  
> **示例输入:** `"牛".encode("utf-8")`（即字节串 `b'\xe7\x89\x9b'`）。 
>
> **解释:** 这个函数错误地尝试将序列中的每个单独字节独立解码为 UTF-8，但这会引发 `UnicodeDecodeError`，因为像“牛”这样的非 ASCII Unicode 字符在 UTF-8 中是多字节的（可变长度编码），单独解码它的某一个字节是不合法的。

**(c) Give a two-byte sequence that does not decode to any Unicode character(s).** *Deliverable: An example, with a one-sentence explanation.*

> **Your Answer:**
>
>  **示例:** `b'\xff\xff'` **解释:** 字节 `0xFF` 在 UTF-8 编码规范中永远不是合法的起始字节或延续字节，因此解析器无法将其解码为任何有效的 Unicode 字符。 *(注：另一个常见答案是* `b'\x80\x80'`*，因为它包含两个延续字节，但缺少指示多字节字符开头的起始字节。)*

