## jestAPI整理

| 方法 | 说明 | 注意事项 |
| --- | --- | --- |
| toBe(value) | 相等匹配，一般用来比例字符串、数值、布尔值是否符合预期 | - |
| toEqual | 匹配对象或数组，toEqual是通过递归检查对象或数组的每个字段 | - |
| toBeCloseTo | 检测对具胡toMatch正则表达式的字符串 | - |
| toBeNull | 只匹配null | - |
| toBeUndefined | 只匹配undefined | - |
| toBeDefined | 与toBeUndefined相反 | - |
| toBeTruthy | 匹配任何if语句为真 | - |
| toBeFlsy | 匹配任何f语句为假 | - |
| toThrow | 测试特定的函数抛出的错误 | - |
