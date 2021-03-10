<template>
  <div class="home">
    <!--header-->
    <header class="whole-header-container">
      <section class="header-container flex max-width">
        <div class="logo">
          <img src="@/assets/icons/aerolab-logo.svg" alt="aerolab-logo">
        </div>
        <div class="info-container flex">
          <span class="userName">{{ this.userName }}</span>
          <div class="userPoints flex" :key="this.userPoints">
            <span>{{ this.userPoints }}</span>
            <img src="../assets/icons/coin.svg" alt="coin">
          </div>
          <img src="../assets/icons/kebab.svg" class="kebab" alt="user-options" @click="toggleMenu">
          <div class="menu absolute">
            <ul>
              <li><a href="" @click="showPointsAdder">Add Points <span>(it's free!)</span></a></li>
            </ul>
          </div>
        </div>
      </section>
    </header>

    <!--header-->
    <main>
      <!--banner-->
      <section class="banner-container">
        <div class="text-container max-width flex">
          <h2>Electronics</h2>
        </div>
      </section>

      <!--cards-->
      <section class="cards-container max-width">
        <div class="panel-container flex">
          <div class="products-shown">
            <h4>{{ this.currentN }} of {{ this.nOfProducts }} products</h4>
          </div>
          <hr>
          <div class="sorting-panel flex">
            <span>Sort by:</span>
            <div class="labels">
              <input type="radio" id="new" name="sort" checked>
              <label @click="sortNew" for="new">Most recent</label>
              <input type="radio" id="low" name="sort">
              <label @click="sortLow" for="low">Lowest price</label>
              <input type="radio" id="high" name="sort">
              <label @click="sortHigh" for="high">Highest price</label>
            </div>
          </div>
          <div class="arrows-container flex">
            <button class="prev" @click="getListOfProducts(1, 16, 16)"><img src="../assets/icons/arrow-left.svg" alt=""></button>
            <button class="next" @click="getListOfProducts(2, 16, 32)"><img src="../assets/icons/arrow-right.svg" alt=""></button>
          </div>
        </div>
        <hr class="separator max-width">
        <div class="products-catalog flex">
          <div class="product-card relative" v-for="(card, index) in cards" :key="index">
            <div class="neededCoins absolute" v-show="userPoints < card.cost">
              <span class="flex">You need {{card.cost - userPoints}} <img src="../assets/icons/coin.svg"></span>
            </div>
            <img v-show="userPoints >= card.cost" src="../assets/icons/buy-blue.svg" alt="" class="bag-icon absolute">
            <img :src="card.img.url" alt="">
            <hr>
            <span class="category" v-html="card.category"></span>
            <h3 class="name" v-html="card.name"></h3>

            <div class="price-container flex absolute">
              <img src="../assets/icons/buy-white.svg" alt="" class="bag-icon absolute">
              <div class="flex price"><span>{{ card.cost }}</span><img src="../assets/icons/coin.svg"></div>
              <span href="#" class="redeem" @click="redeemItem(card._id)">Redeem now</span>
            </div>
          </div>
        </div>
        <hr class="separator">
        <div class="bottom-panel flex">
          <div class="products-shown">
            <h4>{{ this.currentN }} of {{ this.nOfProducts }} products</h4>
          </div>
          <div class="arrows-container flex">
              <button class="prev" @click="getListOfProducts(1, 16, 16)"><img src="../assets/icons/arrow-left.svg" alt=""></button>
              <button class="next" @click="getListOfProducts(2, 16, 32)"><img src="../assets/icons/arrow-right.svg" alt=""></button>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name:'Challenge',
  data(){
    return{
      userName:'',
      userPoints:0,
      currentN:16,
      nOfProducts:32,
      cards:[],
    }
  },
   methods:{
    toggleMenu(){
      let menu = document.querySelector('.menu')
      menu.classList.toggle('open')
    },
    getInfo() {
      axios
      .get('https://coding-challenge-api.aerolab.co/user/me', {
        headers: {
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MDQyYjI2NjdlNzE4NzAwMjBlMzhmNDUiLCJpYXQiOjE2MTQ5ODM3ODJ9.B9YJoKCfxOiZt8PnCpMJVRG3bwB-uoWVZRKUrM0DV5U'
        }
      })
      .then(response => {
        console.log(response.data)
        let data = response.data
        this.userName = data.name
        this.userPoints = data.points
      })
      .catch(err => {
        console.log(err)
      })
    },
    getListOfProducts(currentPage, itemsPerPage, itemsShown){
      axios
      .get('https://coding-challenge-api.aerolab.co/products', {
        headers: {
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MDQyYjI2NjdlNzE4NzAwMjBlMzhmNDUiLCJpYXQiOjE2MTQ5ODM3ODJ9.B9YJoKCfxOiZt8PnCpMJVRG3bwB-uoWVZRKUrM0DV5U'
        }
      })
      .then(response => {
        let data = response.data
        let page = currentPage || 1,
        per_page = itemsPerPage || 10,
        offset = (page - 1) * per_page,

        paginatedItems = data.slice(offset).slice(0, itemsPerPage),
        total_pages = Math.ceil(data.length / per_page);
        
        let highSorter = document.querySelector('#high')
        let lowSorter = document.querySelector('#low')

        this.currentN = itemsShown
        this.cards = paginatedItems

        
        if(highSorter.checked === true){
         this.cards.sort((a, b) => (a.cost < b.cost) ? 1 : -1)
        } else if(lowSorter.checked === true){
          this.cards.sort((a, b) => (a.cost > b.cost) ? 1 : -1)
        }


      })
      .catch(err => {
        console.log(err)
      })
    },
    showPointsAdder(e){
      e.preventDefault();
      
      let pointsAdder = document.querySelector('.whole-pointAdder-container')
      pointsAdder.style.display = 'block'
    },
    redeemItem(ID){
      var request = new XMLHttpRequest();
      let modal = document.querySelector('.whole-modal-container')
      let modalImg = document.querySelector('.whole-modal-container .card img')
      let modalTitle = document.querySelector('.whole-modal-container .card h2')
      let modalMsg = document.querySelector('.whole-modal-container .card .message')
      let modalBtn = document.querySelector('.whole-modal-container .card a')

      request.open('POST', 'https://coding-challenge-api.aerolab.co/redeem');

      request.setRequestHeader('Content-Type', 'application/json');
      request.setRequestHeader('Accept', 'application/json');
      request.setRequestHeader('Authorization', 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MDQyYjI2NjdlNzE4NzAwMjBlMzhmNDUiLCJpYXQiOjE2MTQ5ODM3ODJ9.B9YJoKCfxOiZt8PnCpMJVRG3bwB-uoWVZRKUrM0DV5U');

      request.onreadystatechange = function () {
        if (this.readyState === 4) {
          console.log('Status:', this.status);
          console.log('Headers:', this.getAllResponseHeaders());
          console.log('Body:', this.responseText);

          if(this.status >= 100 && this.status <= 299){
            modal.style.display = 'block'
            modalImg.src = require('../assets/icons/happyFace.svg')
            modalTitle.innerHTML = 'Congratulations!'
            modalMsg.innerHTML = "You've successfully redeemed this item"
            modalBtn.innerHTML = "Yay! Let's keep shopping"
          }
          if(this.status >= 400 && this.status < 600 ){
            modal.style.display = 'block'
            modalImg.src = require('../assets/icons/sadFace.svg')
            modalTitle.innerHTML = "I'm so sorry"
            modalMsg.innerHTML = "You don't have enough points"
            modalBtn.innerHTML = "It's gonna be a sad Christmas for Johnny"
          }
        }
      };

      var body = {
        'productId': `${ID}`
      };

      request.send(JSON.stringify(body));
    },
    sortLow(){
      let cards = this.cards
      cards.sort((a, b) => (a.cost > b.cost) ? 1 : -1)
    },
    sortHigh(){
      let cards = this.cards
      cards.sort((a, b) => (a.cost < b.cost) ? 1 : -1)
    },
    sortNew(){
      /*as there is no date for each product, but the 'most recent' button
      should do something, I choose to sort randomly the array to mock the function*/
      let array = this.cards
      array.sort(() => Math.random() - 0.5);
    },
  },
  mounted(){
    this.getInfo()
    this.getListOfProducts(1, 16, 16)
  }
}
</script>

<style lang="scss" scoped>
@import '../variables.scss';

header{
  position:fixed;
  width:100%;
  background: $white;
  z-index: 10;
  box-shadow: $boxShadow;
  max-height: 5rem;
  .header-container{
    padding:1rem;
    justify-content: space-between;
    

    .info-container{
      font-weight: 900;
      .userPoints{
        background:$softGrey;
        padding:.4rem 1rem;
        border-radius: 200px;
        margin-left: 1rem;

        img{
          margin-left: .5rem;
        }
      }
      .kebab{
        width:1rem;
        margin-left: .5rem;
        cursor:pointer;
      }
      .menu{
        background:$white;
        padding:1rem 1.2rem;
        bottom:-5rem;
        right:1rem;
        box-shadow: $boxShadow;
        display:none;

        li{
          margin: 1rem 0;
            text-align: right;

          a{
            font-size: 1.2rem;
            color:$black;
          }

          span{
            color:$grey
          }
        }
      }
    }
  }
}

main{
  padding-top: 5rem;

  .banner-container{
    background:url('../assets/imgs/header-x2.png') no-repeat 80% center/cover;
    min-height: 25rem;
    padding:2rem 1rem;
    display:flex;
    align-items: flex-end;
    justify-content: flex-start;
    color:$white;

    .text-container{
      align-items: flex-end;
      justify-content: flex-start;
      width: 100%; height:100%; 
      h2{
        font-weight: 900;
      }
    }

  }

  .cards-container{

    .panel-container{
      flex-direction: column;
      padding:2rem 1rem;
      color:$grey;

      hr{
        display:none;
        @media(min-width: 768px){
          display:block;
        }
      }
      .products-shown{
        margin:0 0 1rem 0;

        @media(min-width: 768px){
          margin:0 1rem 0 0;
        }
      }
      .sorting-panel{
        flex-direction: column;
        margin:0 0 1rem 0;
        flex-wrap: wrap;      
        flex:2;  
        @media(min-width: 768px){
          margin:0 0 0 1rem;
        }


        .labels{
          display:flex;
          flex-direction: column;
          label{
            padding:.3rem 1rem;
            border-radius: 200px;
            color:$grey;
            background:$softGrey;
            cursor:pointer;
            text-align: center;
            margin:.5rem 0;

            @media(min-width: 768px){
              margin:0 .5rem;
            }
          }
        }
        input{
          display:none;
        }
        input:checked + label{
          background:$lightBlue;
          color:$white;
        }
      }

      .arrows-container{
        button{
          background:none;
          border:none;
          outline:none;
          cursor:pointer;
          margin: 0 .5rem;
        }
      }
    }
    .separator{
      width:85%;
      margin-bottom: 2rem;
      opacity:.6;
      @media(min-width: 768px){
        width:100%;
      }
    }
    .products-catalog{
      flex-wrap: wrap;
      padding-bottom: 2rem;
      justify-content: space-evenly;

      .product-card{
        margin:.5rem;
        padding:1rem;
        min-width: 20rem;
        box-shadow: $boxShadow;
        overflow: hidden;
        transition:.4s all;

        .neededCoins{
          background:$lowOpacityGrey;
          right:1rem; top:1rem;
          border-radius: 200px;
          padding:.3rem .8rem;
          color:$white;

          img{
            margin-left: .5rem;
          }
        }

        hr{
          margin-bottom: 1rem;
          border:1px solid $softGrey;
        }

        .category{
          color:$grey
        }

        .bag-icon{
          right:1rem; top:1rem;
        }

        .price-container{
          background:rgba(15, 206, 219,.8);
          height:100%; width:100%;
          top:0;
          left:0;         
          flex-direction: column;
          justify-content: center;
          transition:.4s all;
          transform:translateY(100%);

          .price{
            margin-bottom: 1rem;
            span{
              color:$white;
              font-size: 2rem;
            }
            img{
              margin-left: 1rem;
            }
          }

          .redeem{
            background:$white;
            padding:1rem 3rem;
            border-radius: 200px;
            color:$grey;
            font-weight: 700;
            font-size: 1.3rem;
            cursor:pointer;
          }
        }
      }
      .product-card:hover{
        box-shadow: $boxShadowHover;
        transform: translateY(-1rem);
      }

      .product-card:hover .price-container{
        transform: translateY(0) !important;
      }
    }
    .bottom-panel{
      flex-direction: column;
      justify-content: space-between;
      color:$grey;

      .arrows-container{
        margin-bottom: 2rem;
        button{
          background:none;
          border:none;
          outline:none;
          cursor:pointer;
          margin: 0 .5rem;
        }
      }
    }
  }
}

@media(min-width: 768px){
  main{
    .banner-container{
      .text-container{
        h2{
          font-size: 3rem;
        }
      }
    }

    .cards-container{
      .panel-container{
        flex-direction: row;
        justify-content: space-between;
        margin-top: 1rem;

        hr{
          height:2rem;
          border: 1px solid $softGrey;
        }

        .sorting-panel{
          .labels{
            flex-direction: row;

            label{
              margin: 0 .5rem;
            }
          }
        }
      }
      .bottom-panel{
        flex-direction: row;
        margin-bottom: 2rem;

        .arrows-container{
          margin-bottom: 0;
        }
      }
    }
  }
}
@media(min-width: 1200px){
  main{

    .banner-container{
      .text-container{
        h2{
          font-size: 4rem;
        }
      }
    }

    .cards-container{
      .panel-container{
        .sorting-panel{
          flex-direction: row;
        }
      }
    }
  }
}
</style>