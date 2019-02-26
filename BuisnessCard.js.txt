class Card extends React.Component {
constructor(props){
        super(props);
}

render(){
        return(
        <div>
        <img style={{width:75}} src={this.props.picture} />
    <div style={{display:'inline-block', margin:10}}>
    <div style={{fontSize:'1.25em',fontWeight:'bold'}}>{this.props.cardName}</div>
    <div>{this.props.companyName}</div>
    </div>
    </div>
  );
}
}

class Form extends React.Component {
constructor(props) {
        super(props);

  this.state = {
    userName :''
  }
  this.handleSubmit = this.handleSubmit.bind(this);
}

handleSubmit(e) {
          e.preventDefault();
   console.log(this.state.userName);
   axios.get(`https://api.github.com/users/${this.state.userName}`).then(
        (resp) => {
                this.props.onSubmit(resp.data);
    });
};

render() {
        return(
                <form onSubmit = {this.handleSubmit}>
        <input type="text"placeholder = "github UserName"
        value ={this.state.userName}
        onChange = {(event) => {this.setState({userName : event.target.value});
        console.log(this.state)}}
        required/>
        <button type="submit">Add User </button>
        </form>

    );
  }
}

class CardList extends React.Component{
        constructor(props){
  super(props);
  }
        render(){
        return(
        <div>
   {this.props.cardInfo.map(car => <Card cardName={car.name} picture={car.avatar_url} companyName={car.company} />)}
      </div>
  )
  }
}

class App extends React.Component {
        state = {
                cards : [
  ]
  }

  addNewCard = (cardInfo) => {
                this.setState(prevState => ({
                cards : prevState.cards.concat(cardInfo)
      }))
  }

        render(){
        return(
                <div>
                <Form onSubmit = {this.addNewCard}/>
        <CardList  cardInfo={this.state.cards}/>
        </div>
    );
  }
}

ReactDOM.render(<App/>,mountNode);
