<template>
    <section class="whole-modal-container">
        <section class="modal-container">
            <div class="card flex relative">
                <img src="" alt="face" class="face">
                <h2></h2>
                <p class="message"></p>
                <a href=""></a>
            </div>
        </section>
    </section>
</template>

<script>
export default {
    name:'Modal',

}
</script>

<style scoped lang="scss">
@import '../variables.scss';
.whole-modal-container{
    height:100%; width:100%;
    position:fixed;
    top:0; left:0;
    background:rgba(0,0,0,.5);
    z-index: 50;
    display:none;

    .modal-container{

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

            .face{
                margin-bottom: 1rem;
            }

            p{
                color:$grey;
                text-align: center;
                font-size: 1.1rem;
                margin:1rem 0;
            }

            a{
                background:$lightBlue;
                padding:.5rem 1.3rem;
                color:$white;
                font-weight: 700;
                text-align: center;
                font-size: 1.2rem;
                border-radius: 200px;
                cursor:pointer;
            }
        }
    }
}
</style>