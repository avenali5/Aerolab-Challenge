<template>
    <section class="whole-pointAdder-container">
        <section class="pointAdder-container">
            <div class="card flex relative">
                <span class="cross absolute" @click="closeModal">✗</span>
                <p class="message">How many points would you like to add to your user?</p>
                <div class="buttons flex">
                    <a @click="addPoints(1000)" class="first-opt">1000</a>
                    <a @click="addPoints(5000)" class="second-opt">5000</a>
                    <a @click="addPoints(7500)" class="third-opt">7500</a>
                </div>
                <div class="loader absolute flex">
                    <div class="dot1"></div>
                    <div class="dot2"></div>
                    <div class="dot3"></div>
                </div>
                <div class="reloader">
                    <a href="" @click="closeModal"></a>
                </div>
            </div>
        </section>
    </section>
</template>

<script>
export default {
    name:'AddPoints',
    methods:{
        addPoints(amount){
            var request = new XMLHttpRequest();
            let buttons = document.querySelector('.buttons')
            let option1 = document.querySelector('.first-opt') 
            let option2 = document.querySelector('.second-opt') 
            let option3 = document.querySelector('.third-opt') 
            let loader = document.querySelector('.loader')
            let cardMsg = document.querySelector('.whole-pointAdder-container .card .message')

            request.open('POST', 'https://coding-challenge-api.aerolab.co/user/points');

            request.setRequestHeader('Content-Type', 'application/json');
            request.setRequestHeader('Accept', 'application/json');
            request.setRequestHeader('Authorization', 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MDQyYjI2NjdlNzE4NzAwMjBlMzhmNDUiLCJpYXQiOjE2MTQ5ODM3ODJ9.B9YJoKCfxOiZt8PnCpMJVRG3bwB-uoWVZRKUrM0DV5U');

            setTimeout(function(){
                option1.style.transform = 'translateY(3rem)'
                option3.style.transform = 'translateY(-3rem)'
                option1.style.opacity = '0'
                option2.style.opacity = '0'
                option3.style.opacity = '0'
                loader.style.opacity = '1'
            }, 500)


            request.onreadystatechange = function () {
            if (this.readyState === 4) {
                console.log('Status:', this.status);
                console.log('Headers:', this.getAllResponseHeaders());
                console.log('Body:', this.responseText);
                loader.style.opacity = '0'
                buttons.style.display = 'none'

                if(this.status >= 100 && this.status <= 299){
                    cardMsg.innerHTML = 'Points added successfully!'
                }
                if(this.status >= 400 && this.status < 600 ){
                    cardMsg.innerHTML = 'Agh, we got a problem, try again later please'
                }
                //reload page after charging points
                setTimeout(function(){location.reload()}, 1000)
            }
            };

            var body = {
            'amount': amount
            };

            request.send(JSON.stringify(body));
        },
        closeModal(){
            let modal = document.querySelector('.whole-pointAdder-container')
            modal.style.display = 'none'
        }
    }
}
</script>

<style scoped lang="scss">
@import '../variables.scss';
.whole-pointAdder-container{
    height:100%; width:100%;
    position:fixed;
    top:0; left:0;
    background:rgba(0,0,0,.5);
    z-index: 50;
    display: none;

    .pointAdder-container{

        .card{
            flex-direction: column;
            justify-content: center;
            background:$white;
            width:80%;
            max-width: 25rem;
            left:50%;
            top:10rem;
            transform:translateX(-50%);
            padding:2rem;

            .cross{
                top:1rem;right:1rem; cursor: pointer;
            }

            p{
                color:$grey;
                text-align: center;
                font-size: 1.1rem;
            }
            .buttons{
                flex-direction: column;
                margin-top: 1rem;
                a{
                    background:$lightBlue;
                    padding:.5rem 1.3rem;
                    color:$white;
                    font-weight: 700;
                    text-align: center;
                    font-size: 1.2rem;
                    border-radius: 200px;
                    cursor:pointer;
                    transition:.4s all;
                    margin:.5rem 0;
                }
            }

            .loader{
                justify-content: center;
                bottom:6.7rem;
                opacity:0;
                div{
                    background:$black;
                    width:.5rem; height:.5rem;
                    border-radius: 200px;
                    margin:0 .5rem;
                }
                @for $j from 1 through 3{
                    .dot#{$j}{
                        animation: jump 1s .2s*$j linear infinite;
                    }
                }

            }
        }
    }
}
@keyframes jump {
	0%, 60%, 100% {
		transform: initial;
	}

	30% {
		transform: translateY(-15px);
	}
}
</style>