

```js
import React, { PropTypes } from 'react'
import marked from 'marked'
class Marked extends React.Component {
  render () {
    let content=marked('_asd_')
    return(
      <div>
        <div dangerouslySetInnerHTML={{__html:content}} />
      </div>
    )
  }
}

```
