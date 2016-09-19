import React, { PropTypes } from 'react';

import {searchGit} from './utils/helpers'
class About extends React.Component {
  constructor(){
    super();
    this.state={
      data:{},
      wait:true,
      inputValue:'username'
    }
  }
  componentDidMount(){

  }
  handleInput(e){
    let value=e.target.value;
    this.setState({
      inputValue:value
    })
  }
  handleClick(){
    let name=this.state.inputValue;
    searchGit(name)
      .then( (recData) => {
        console.log(recData);
        this.setState({
          data:recData.getData,
          wait:false
        })
      })
  }
  render () {
    let styles={
      root:{
        maxWidth:'700px',
        margin:'0 auto',
        border:'1px solid teal',
        textAlign:'center'
      }
    }
    let gitInfo=(
      <div>
        <h3>{this.state.data.name}</h3>
        <img src={this.state.data.avatar_url}/>
      </div>
    )
    return(
      <div style={styles.root}>
        <input type='text' value={this.state.inputValue} onChange={this.handleInput.bind(this)}/>
        <button onClick={this.handleClick.bind(this)}>搜索</button><br/>
        {  this.state.wait ? '正在获取数据' : gitInfo }
      </div>
    )
  }
}

export default About;
