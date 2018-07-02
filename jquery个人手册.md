#jquery个人手册
## 操作 Manipulation
### DOM Insertion, Outside
#### .insertAfter( target )
##### 用例
```javascript
$( '#b' ).insertAfter( '.a' );
```
是将 a 插入到 b 的后面
`返回值`是插入的b (因为'.a'可能不止一个)