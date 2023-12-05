```jsx
const [questionBankList,setQuestionBankList] = useState([
    {
      key: '1',
      title: '标题描述标题描述标题描述标题描述标题描述标题描述标题描述标题描述标题描述标题描述标题描述',
      answer: 'A',
      analysis: '解题思路',
      options: [
        {
          key: 1,
          content: '选项一'
        }, {
          key: 2,
          content: '选项二'
        }, {
          key: 3,
          content: '选项三'
        }
      ]
    }, {
      key: '1',
      title: '标题描述',
      answer: 'A',
      analysis: '解题思路',
      options: [
        {
          key: 1,
          content: '选项一'
        }, {
          key: 2,
          content: '选项二'
        }, {
          key: 3,
          content: '选项三'
        }
      ]
    }
  ])
  
  // 此处需要浅拷贝，不然相当于直接操作了源数据，导致不能刷新数据
  const deleteItem = (index) => {
    let tmp = [...questionBankList]
    tmp.splice(index,1)
    setQuestionBankList(tmp)
    console.log(questionBankList)
  }
```
