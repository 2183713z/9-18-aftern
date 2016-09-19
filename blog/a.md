

```js
class Marked extends React.Component {
  render () {
    // console.log(marked(' # I am using __markdown__.'));
    let content=marked('_asd_')
    return(
      <div>
        <div dangerouslySetInnerHTML={{__html:content}} />
      </div>
    )
  }
}

```
