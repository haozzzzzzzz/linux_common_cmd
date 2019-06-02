短链生成算法

- 参考https://segmentfault.com/a/1190000012088345

原理是对md5值进行分段，然后转成16进制数值，取段内的数值作为映射字母表索引，然后生成短链。



改进为利用63个字符库生成8位字符后的算法示例

```go
/** implementation of short url algorithm **/
/*
8888 7777 6666 5555 4444 3333 2222 1111
221111
331111
..
xx8888 63个字符表（11 1111）需要原数值位移补足两位 即二进制 原来数值加上 00 0000 ~ 11 0000(48)
*/
func ShortUrlTransform(longURL string) ([4]string, error) {
	md5Str := getMd5Str(longURL)
	//var hexVal int64
	var tempVal int64
	var result [4]string
	var tempUri []byte
	for i := 0; i < 4; i++ {
		tempSubStr := md5Str[i*8 : (i+1)*8] // 8byte
		hexVal, err := strconv.ParseInt(tempSubStr, 16, 64)
		if err != nil {
			return result, nil
		}
		tempVal = hexVal
		//tempVal = int64(VAL) & hexVal
		var index int64
		tempUri = []byte{}
		for i := 0; i < 8; i++ { // 生成8位字符
			if i == 7 { // 最后一次，补足两位
				tempVal += num.RandInt64(0, 49) // [0, 49)
			}

			index = INDEX & tempVal // 得出索引0~63（占6位）
			tempUri = append(tempUri, alphabet[index])
			tempVal = tempVal >> 4 // 每4位取一个索引
		}
		result[i] = string(tempUri)
	}
	return result, nil
}
```

