# Sticky-Notes

you can download the project from here 

https://drive.google.com/file/d/1W6yNPifuxxdCf6d2OuCWscMtYSD5w6ts/view?usp=sharing

you can install reactjs and and nodejs 

you can use visual studio code for editing and coding 

you can run html through terminal that npm start 


---> Or you can simple run this html file.



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@lamflam/react-draggable@4.0.4/web/react-draggable.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        .note{
            width:250px;
            height:150px;
            background-color:yellow;
            padding:15px;
            border: 1px solid orange;
            box-shadow: 2px 6px 12px rgba(0,0,0,0.2);
            position: absolute;
        }

    </style>
</head>
<body>
<div id="react-container"></div> 
<script type="text/babel">

class Note extends React.Component{

    constructor(props){
        super(props);
        this.state={
            editing :false 
        }
    }
    componentWillMount=()=>{
        this.style={
            right:this.randomBetween(0,window.innerWidth-150,'px'),
            top:this.randomBetween(0,window.innerHeight-150,'px')
        }
    }
    randomBetween=(x,y,s)=>{
        return (x+Math.ceil(Math.random()*(y-x)))+s

    }
    edit = () => {
        this.setState({editing : true})
    }
    save = () => {
        this.props.onChange(this.refs.newText.value,this.props.id)
        this.setState({editing:false})
    }

    delete = ( id ) => {
        this.props.onRemove(this.props.id)
    }

    renderForm = () => {
        return(
            <div class="note" style={this.style}>
                <textarea ref="newText"></textarea> 
                <button onClick={this.save}>Save</button>
            </div>
        )
            
    }

    renderDisplay = () =>{
        return(
            <div className ="note" style={this.style}>
                <p>{this.props.children}</p>
                <span>
                    <button onClick={this.edit}>Edit</button>
                    <button onClick={this.delete}>X</button>
                </span>
            </div>

        )
    }
    render(){
      return(<ReactDraggable>{
        (this.state.editing) ? this.renderForm() : this.renderDisplay()
        }</ReactDraggable>)
    }
          
}
ReactDOM.render(<Note>Hello</Note>,document.getElementById('react-container'))

class Board extends React.Component{

    constructor(props){
        super(props);
        this.state = {
            notes: []
        }
    }
    
    nextId=()=>{
        this.uniqueId=this.uniqueId || 0
        return this.uniqueId++
    }
    add =(text)=>{
        var notes=[
            ...this.state.notes,
            {
                id:this.nextId(),
                note:text
            }

        ]
        this.setState({notes})
    }
    update = (newText,id) => {
        var notes =this.state.notes.map(
            note => (note.id !== id ) ? 
                note:
                {
                    ...note,
                    note:newText
                }
        )
        this.setState({notes})
    }
    remove = ( id ) =>{
        var notes=this.state.notes.filter(note => note.id !== id)
        this.setState({notes})
    }
    eachNote = (note) =>{
        return (<Note key={note.id}
                id={note.id}
                onChange={this.update}
                onRemove={this.remove} >{note.note}</Note>)
    }
    render(){
        return(
            <div className="board">
                {this.state.notes.map(this.eachNote)}
                <button onClick={()=>this.add()}>+</button>
            </div>
        )
    }
}

ReactDOM.render(<Board/>,document.getElementById('react-container'))
</script>
</body>
</html>
