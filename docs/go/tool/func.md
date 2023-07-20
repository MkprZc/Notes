```go
/**
 * 查找函数在字符串中的位置
 * @param str 字符串
 * @param fn 函数开始字符串 (例如: `tan(` )
 * @return []int 切片索引每两个为函数的开始和结束位置
 */
func findFunctionPosition(str string, fn string) ([]int, error) {
	length := len(str)
	fnLength := len(fn)
	if length == 0 || fnLength == 0 {
		return nil, errors.New("str or fn is zero")
	}

	// 函数的位置切片
	positionSlice := make([]int, 0)

	for i := 0; i < length; i++ {
		if i+fnLength >= length {
			break
		}

		if str[i:i+fnLength] == fn {
			left := 1
			right := 0

			for j := i + fnLength; j < length; j++ {
				s := str[j : j+1]
				if s == "(" {
					left += 1

				} else if s == ")" {
					right += 1

					if left == right {
						positionSlice = append(positionSlice, i, j)
						i = j
						break
					}
				}
			}
		}

	}

	return positionSlice, nil
}
```