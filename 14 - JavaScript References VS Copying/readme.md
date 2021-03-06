# 14 - JavaScript References VS Copying

## call by value(share)
JS 中 `String`、`Number`、`Boolean`、`null`、`undefined`、`NaN` 為傳值的特性
即當有新的值被賦予到變數時，會在記憶體新增一個位址記錄此一個值，變數在連結此一個值的位址。

```javascript
let a = "A"
let b = a
a = "B"

console.log(a, b) // "B", "A"
```

<table>
  <tr>
    <td width="50%">
      <ol>
      <pre><code>a = "A"</code></pre>
        <li>記憶體 0x01 紀錄值 "A"</li>
        <li>變數 a 連結至位址 0x01</li>
        <pre><code>let b = a</code></pre>
        <li>變數 b 連結至位址 0x01</li>
        <pre><code>a = "B"</code></pre>
        <li>記憶體 0x04 紀錄值 "B"</li>
        <li>變數 a 連結至位址 0x04</li>
      </ol>
    </td>
    <td>
      <table>
        <thead>
          <tr>
            <th>變數</th>
            <th>記憶體</th>
            <th>值</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td></td>
            <td>0x01</td>
            <td>"A"</td>
          </tr>
          <tr>
            <td>a</td>
            <td>0x02</td>
            <td>0x04</td>
          </tr>
          <tr>
            <td>b</td>
            <td>0x03</td>
            <td>0x01</td>
          </tr>
          <tr>
            <td></td>
            <td>0x04</td>
            <td>"B"</td>
          </tr>
        </tbody>
      </table>
    </td>
  </tr>
</table>

---

```javascript
let x = "A"
let y = "A"
let z = "A"

y = "B", z = "C", x += y, x += z
console.log(x, y, z)  // "ABC", "B", "C"
```

<table>
  <tr>
    <td width="50%">
      <ol>
      <pre><code>let x = "A"; let y = "A"; let z = "A"</code></pre>
        <li>記憶體 0x01 紀錄值 "A"</li>
        <li>變數 x 連結至位址 0x01</li>
        <li>變數 y 連結至位址 0x01</li>
        <li>變數 z 連結至位址 0x01</li>
        <pre><code>y = "B"</code></pre>
        <li>記憶體 0x05 紀錄值 "B"</li>
        <li>變數 y 連結至位址 0x05</li>
        <pre><code>z = "C"</code></pre>
        <li>記憶體 0x06 紀錄值 "C"</li>
        <li>變數 z 連結至位址 0x06</li>
        <pre><code>x += y</code></pre>
        <li>x += y 等於 x = x + y ; x + y = "AB" 紀錄於 記憶體 0x07</li>
        <li>變數 ㄌ 連結至位址 0x07</li>
        <pre><code>x += z</code></pre>
        <li>x = x + y = "ABC" 紀錄於 記憶體 0x08</li>
        <li>變數 x 連結至位址 0x08</li>
      </ol>
    </td>
    <td>
      <table>
        <thead>
          <tr>
            <th>變數</th>
            <th>記憶體</th>
            <th>值</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td></td>
            <td>0x01</td>
            <td>"A"</td>
          </tr>
          <tr>
            <td>x</td>
            <td>0x02</td>
            <td>0x06</td>
          </tr>
          <tr>
            <td>y</td>
            <td>0x03</td>
            <td>0x04</td>
          </tr>
          <tr>
            <td>z</td>
            <td>0x04</td>
            <td>0x01</td>
          </tr>
          <tr>
            <td></td>
            <td>0x05</td>
            <td>"B"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x06</td>
            <td>"C"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x07</td>
            <td>"AB"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x08</td>
            <td>"ABC"</td>
          </tr>
        </tbody>
      </table>
    </td>
  </tr>
</table>
<table>

## call by references
觀念與 call by value 相同，但陣列與物件內的值也對應的一個記憶體位址。

### 陣列
```javascript
const fruit1 = ["Apple", "Banana", "Orange", "Mango"]
const fruit2 = fruit1
fruit2[0] = "Pineapple"
console.log(fruit1)  // ["Pineapple", "Banana", "Orange", "Mango"]
```

<table>
  <tr>
    <td>
      <ol>
        <pre><code>const fruit1 = ["Apple", "Banana", "Orange", "Mango"]</code></pre>
        <li>記憶體 0x01 新增紀錄值 "Apple"</li>
        <li>記憶體 0x02 新增紀錄值 "Banana"</li>
        <li>記憶體 0x03 新增紀錄值 "Orange"</li>
        <li>記憶體 0x04 新增紀錄值 "Mango"</li>
        <li>陣列為 [0x01, 0x02, 0x03, 0x04] 的集合，紀錄於 0x05</li>
        <li>陣列 fruit2 指向 0x05</li>
        <pre><code>const fruit2 = fruit1</code></pre>
        <li>陣列 fruit2 指向 0x05</li>
        <pre><code>fruit2[0] = "Pineapple"</code></pre>
        <li>記憶體 0x08 新增紀錄值 "Pineapple"</li>
        <li>陣列 fruit2 (0x05) 的序列 0 指向並更為 0x08</li>
      </ol>
    </td>
    <td>
      <table>
        <thead>
          <tr>
            <th>變數</th>
            <th>記憶體</th>
            <th>值</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td></td>
            <td>0x01</td>
            <td>"Apple"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x02</td>
            <td>"Banana"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x03</td>
            <td>"Orange"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x04</td>
            <td>"Mango"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x05</td>
            <td>[0x08, 0x02, 0x03, 0x04]</td>
          </tr>
          <tr>
            <td>fruit1</td>
            <td>0x06</td>
            <td>0x05</td>
          </tr>
          <tr>
            <td>fruit2</td>
            <td>0x07</td>
            <td>0x05</td>
          </tr>
          <tr>
            <td></td>
            <td>0x08</td>
            <td>"Pineapple"</td>
          </tr>
        </tbody>
      </table>
    </td>
  </tr>
</table>

### 物件
```javascript
const obj = {
  object: "Object",
  string: {
    name: "String",
  },
  null: {
    name: "Null",
  }
}
const obj_2 = obj

obj_2.object = "Array"
obj_2["null"] = "Null"
obj_2["string"]["name"] = "Number"

console.log(obj)
// {
//  object: "Array"
//  string: {name: "string", type: "Number"}
//  null: "Null"
// }
```

<table>
  <tr>
    <td>
      <ol>
        <pre><code>const obj</code></pre>
        <li>記憶體 0x01 新增紀錄值 "Object"</li>
        <li>記憶體 0x02 新增紀錄值 "String"</li>
        <li>記憶體 0x03 新增紀錄值 "Null"</li>
        <li>key object 的值指向 0x01</li>
        <li>記憶體 0x05 新增紀錄值 {name: 0x02}</li>
        <li>key string 的值指向 0x05</li>
        <li>記憶體 0x07 新增紀錄值 {name: 0x03}</li>
        <li>key null 的值指向 0x07</li>
        <pre><code>const obj_2 = obj</code></pre>
        <li>物件 obj_2 指向 0x08</li>
        <pre><code>obj_2.object = "Array"</code></pre>
        <li>記憶體 0x10 新增紀錄值 "Array"</li>
        <li>0x04 object 的值指向 0x10 </li>
        <pre><code>obj_2["null"] = "Null"</code></pre>
        <li>記憶體 0x11 新增紀錄值 "Null"</li>
        <li>0x08 null 的值指向 0x11</li>
        <pre><code>obj_2["string"]["name"] = "Number"</code></pre>
        <li>記憶體 0x12 新增紀錄值 "Number"</li>
        <li>0x06 string => string: 指向 0x12</li>
      </ol>
    </td>
    <td>
      <table>
        <thead>
          <tr>
            <th>變數</th>
            <th>記憶體</th>
            <th>值</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td></td>
            <td>0x01</td>
            <td>"Object"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x02</td>
            <td>"String"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x03</td>
            <td>"Null"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x04</td>
            <td>object: 0x10</td>
          </tr>
          <tr>
            <td></td>
            <td>0x05</td>
            <td>{name: 0x02}</td>
          </tr>
          <tr>
            <td></td>
            <td>0x06</td>
            <td>string: 0x05</td>
          </tr>
          <tr>
            <td></td>
            <td>0x07</td>
            <td>{name: 0x03}</td>
          </tr>
          <tr>
            <td></td>
            <td>0x08</td>
            <td>null: 0x07</td>
          </tr>
          <tr>
            <td>obj</td>
            <td>0x08</td>
            <td>{0x04, 0x06, 0x08}</td>
          </tr>
          <tr>
            <td>obj_2</td>
            <td>0x09</td>
            <td>{0x04, 0x06, 0x08}</td>
          </tr>
          <tr>
            <td></td>
            <td>0x10</td>
            <td>"Array"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x11</td>
            <td>"Null"</td>
          </tr>
          <tr>
            <td></td>
            <td>0x12</td>
            <td>"Number"</td>
          </tr>
        </tbody>
      </table>
    </td>
  </tr>
</table>

## 陣列 copy 的方法

1. `Arr1 = Arr2.slice()`
1. `Arr2 = [].concat(Arr1)`
    - 陣列合併並產新一個新陣列。
1. `Arr2 = [...Arr1]`
    - ES6 的展開。

## 物件 copy 的方法

1. `newObj = Object.assign({}, obj)`
    - 第一個參數可為 一個物件 or 空物件
    - 第二個參數的物件會覆蓋第一個參數物件的內容
    - 此方式為 shallow copy
1. `newObj = JSON.pares(JSON.stringify(obj))`
    - 此方式可達到 deep copy 的效果
    - 如物件內有 `function()` ， copy 後的物件內部 `function()` 無法使用。