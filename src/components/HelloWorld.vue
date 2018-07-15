<template>
 <div id="app">
          <div id="leftSide">
             <div class="form-group">
                <label for="name">Your Name</label>
                <input id="name" name="name" type="text" required  class="form-control" placeholder="Your name" v-model="name">           
                <small id="yourName" class="form-text text-muted" required>Please provide your name.</small>
             </div>
             <div class="form-group">
                <label for="adult"># of Adults</label>
                <input id="adult" name="adult" class="form-control" type="text" required placeholder="# of Adults" v-model.number="currentAdltCnt" v-on:change="checkVal">
                <small id="numberOfAdule" class="form-text text-muted">How many adults in your group?</small>
             </div>
             <div class="form-group">
                <label for="child"># of Children</label>
                <input id="child" name="child" type="text" required class="form-control" placeholder="# of Children" v-model.number="currentChldCnt" v-on:change="checkVal">
                <small id="nmberOfChildren" class="form-text text-muted">How many childres in your group?</small>           
             </div> 
              <div class="form-group">
                <button type="button" class="btn btn-primary" v-on:click="buy" v-bind:class="{ disabled:isDisabled }">Buy</button>
                <span id="error"></span>
              </div>   
              <div>
                <ul style="color:red">
                  <li v-for="error in errors">{{error}}</li>
                </ul>
            </div> 
         </div> 
      
     
      <div id="middle" class="box">
        Ticket Buyers
        <ul id="allSells" v-for="buyer in buyers">
          <li>
            <b>Name:</b>{{buyer.buyerName}} &nbsp;&nbsp;<br />
            <b>Adults:</b>&nbsp;{{buyer.totAdults}} <br />
            <b>Children:</b>&nbsp;{{buyer.totChildren}}
          </li>
          <ul>
            <li>${{buyer.purchaseTotal}}</li>
          </ul>
        </ul>
      </div>
      
      <div id="rightSide" class="box">
        Ticket Inventory <br />
        Total Adult Tickets Remaining: {{adultTicketCount}}<br />
        Total Child Tickets Remaining: {{childTicketCount}}
      </div>
            
      <div>
        
      </div>
    </div> 
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      name: "",
      adultTicketCount: 15,
      childTicketCount: 25,
      childTickets: 10.5,
      adultTickets: 5.25,
      buyers: [],
      currentAdltCnt: 0,
      currentChldCnt: 0,
      disableButton: false,
      inStock: false,
      errors: []
    };
  },
  computed: {
    totalPrice: function() {
      return parseInt(this.buyers.totAdults * this.adultTickets); //+parseInt(this.buyers.totChildren*this.childTickets)
      //return (this.currentAdltCnt * this.adultTickets) + (this.currentChldCnt * this.childTickets) <-- WORKS!
    },
    isDisabled() {
      if (this.currentChldCnt <= 0 || this.currentAdltCnt <= 0) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    buy: function() {
      if (this.errors.length === 0) {
        this.childTicketCount -= this.currentChldCnt;
        this.adultTicketCount -= this.currentAdltCnt;
      }

      //Empty the error array or there will be duplicate values
      this.errors.length = 0;

      //Check if all fields are blank
      if (
        this.is_Blank(this.name) &&
        this.currentAdltCnt === 0 &&
        this.currentChldCnt === 0
      ) {
        this.errors.push("All fields are required.");
        return event.preventDefault();
      }

      //Check if # of Adults and # of Children fields are blank
      if (
        !this.is_Blank(this.name) &&
        (this.currentAdltCnt === 0 || this.currentAdltCnt == null) &&
        (this.currentChldCnt === 0 || this.currentChldCnt == null)
      ) {
        this.errors.push("How many child and/or adult tickets would you like?");
        return event.preventDefault();
      }

      if (this.is_Blank(this.name)) {
        this.errors.push("Name can't be blank!");
        return event.preventDefault();
      }
      console.log("hello");
      console.log(typeof this.currentAdltCnt);

      if (typeof this.currentAdltCnt === "string") {
        this.errors.push("There should be no blanks.");
        return event.preventDefault();
      }

      if (typeof this.currentChldCnt === "string") {
        this.errors.push("There should be no blanks.");
        return event.preventDefault();
      }
      /*   
   if (this.adultTicketCount < 0){
        this.errors.push("There are no more adult tickets.");
    return event.preventDefault();
   }
     
   if (this.childTicketCount < 0){
        this.errors.push("There are no more child tickets.");
    return event.preventDefault();
   }   */

      this.buyers.push({
        buyerName: this.name,
        totAdults: this.currentAdltCnt,
        totChildren: this.currentChldCnt,
        purchaseTotal:
          this.currentAdltCnt * this.adultTickets +
          this.currentChldCnt * this.childTickets
        // purchaseTotal: this.totalPrice
      });
    },
    addTickets: function() {
      var totalCost = 0;
      this.buyers.forEach(function(buyer) {
        totalCost = +buyer.totAdults;
      });
    },
    is_Blank: function(input) {
      if (input.length === 0) return true;
      else return false;
    },
    checkVal: function() {
      if (
        this.currentAdltCnt > this.adultTicketCount ||
        this.currentChldCnt > this.childTicketCount
      ) {
        this.errors.push(
          "You are trying to purchase more tickets than we have."
        );
        return event.preventDefault();
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* CSS files add styling rules to your content */
.button.disabled {
  opacity: 0.65;
  cursor: not-allowed;
}

body {
  font-family: "Benton Sans", "Helvetica Neue", helvetica, arial, sans-serif;
  margin: 2em;
  background-color: #ffdead;
}

h1 {
  font-style: italic;
  color: #373fff;
}

#app {
  display: flex;
  flex-wrap: wrap;
}

#app div {
  margin: 2px;
  padding: 5px;
}

#leftSide {
  flex: 1;
  background-color: #e6e6fa;
}

#middle {
  flex: 1;
  background-color: #ffdead;
  order: -1;
}

#rightSide {
  flex: 1;
  background-color: #f0fff0;
}
</style>
