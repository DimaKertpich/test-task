<template>

    <section :dir="checkHebrew" class="task">
            <div class="task__partialData">
                    <div class="task__list">
                       <h3>List</h3>
                    </div>
                <div :key="index" v-for="(user ,index) in arrayUsers" class="task__users">
                    <div class="task__users-data">
                        <p class="task__users-name" >{{ user.name }}</p>
                        <p class="task__users-city" :key="index" v-for="(location ,index) in user">
                            {{ location.city }}
                        </p>
                    </div>
                    <div class="task__users-btn">
                        <p>{{'#' + user.id }}</p>

                        <button @click="showDetalis(index)" class="task__users-btn-view" type="button"> View </button>
                        <button @click="deleteUser(index)" class="task__users-btn-delete" type="button"> Delete </button>
                    </div>


                    <div class="task__users-line"></div>
                </div>  

                <div v-if="activeErr == true" class="task__errText">
                    <p>Sorry, but cannot be less than one user</p>
                </div>  
            </div>

            <div v-if="activeDetalis == true">
                <div :key="index" v-for="(data ,index) in arrayUsers" class="task__details" >
                
                <div v-if="currentInformation == index">
                    <div  class="task__details-name">
                        <h3>{{ data.name }}</h3>
                    </div>
                    <div class="task__details-identifier">
                        <span>{{"id: " + data.id }} |</span>
                        <span>{{"username: " + data.username }} |</span>
                        <span>{{"email: " + data.email }} </span>
                    </div>
                    <div class="task__details-address">
                        <h3>Address</h3>

                        <p>{{ data.address.zipcode + ' ' + data.address.city + ' ' + data.address.street + ' ' + data.address.suite }}</p>
                        
                        <span>Geo:<p>
                            {{"lat: " + data.address.geo.lat + ' - ' + "lng:" +  data.address.geo.lng}}
                        </p></span>                   
                    </div>
                    <div class="task__details-phone">
                        <h3>?מָה עָשָׂה שׁחר</h3>
                        <p>{{ data.phone }}</p>
                    </div>
                    <div class="task__details-website">
                        <h3>Website</h3>
                        <p>{{ data.website }}</p>
                    </div>
                    <div class="task__details-company">
                            <h3>Company</h3>
                            <p>{{ data.company.name }}</p>
                            <span>{{ data.company.catchPhrase }}</span>
                            <br>
                            <span>{{ data.company.bs }}</span>
                        </div>
                    </div>
                
                </div>
            </div>
    
    </section>

</template>

<script>
export default{
    data(){
        return{
            getUsers: [],
            arrayUsers: [],
            activeErr: false,
            currentInformation: 0,
            activeDetalis: false,
            pattern: /[\u0590-\u05FF]/,
            checkLang: null
        }
    },

    computed:{
        checkHebrew(){   
            return this.pattern.test(this.checkLang) ? 'rtl' : 'ltr'
        }
    },

    methods:{
       deleteUser(value){
           if(this.arrayUsers.length != 1){
               this.arrayUsers.splice(value, 1);
           }else{
               this.activeErr = true
           }
       },

       showDetalis(index){
           this.activeDetalis = true
           this.currentInformation = index
       }

    //    checkHebrew(){
    //        let Hebrew = document.querySelector('.task .task__list h3').innerHTML;
    //         console.log(Hebrew)
    //        if(this.pattern.test(Hebrew)){
    //            console.log(1)
    //            return 'rtl'
    //        }else{
    //            console.log(2)
    //            return 'ltr'
    //        }
    //    }

    },

    async beforeMount(){
    // fetch('https://jsonplaceholder.typicode.com/users')
    //   .then(function(responce){
    //     responce.json().then(function(data){
    //       console.log(data)
    //     })
    //   })

    let responce = await fetch('https://jsonplaceholder.typicode.com/users');

        if(responce.ok){
            this.getUsers = await responce.json();
            this.arrayUsers = this.getUsers.slice(0 , 5)
        }


        this.checkLang = document.querySelector('.task .task__list h3').innerHTML;
    },


}

</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');

.task{
    display: flex;
    width: 1366px;
    padding: 0px 70px;
}

.task .task__partialData{
    width: 600px;
    margin-top: 18px;
}

.task .task__partialData .task__list h3{
   
    font-size: 36px;
    font-family: 'Open Sans', sans-serif;
}

.task .task__partialData .task__users{
    position: relative;
    margin-bottom: 20px; 
    display: flex;
    justify-content: space-between;
}

.task .task__partialData .task__users .task__users-data .task__users-name{

    font-size: 18px;
    font-family: 'Open Sans', sans-serif;
    margin: 0;
}

.task .task__partialData .task__users .task__users-data .task__users-city{

    font-size: 14px;
    color: #4F4F4F;
    font-family: 'Open Sans', sans-serif;
    margin: 0;
    padding-top: 1px;
}

.task .task__partialData .task__users .task__users-line{
    position: absolute;
    bottom: -11px;
    left: 0;
    width: 100%;
    height: 1px;
    background: #E0E0E0;
}

.task .task__partialData .task__users .task__users-btn{
    display: flex;
}

.task .task__partialData .task__users .task__users-btn p{
    font-size: 14px;
    color: #4F4F4F;
    font-family: 'Open Sans', sans-serif;
}

.task .task__partialData .task__users .task__users-btn .task__users-btn-view{
    font-size: 14px;
    color: #fff;
    font-family: 'Open Sans', sans-serif;
    background: #4F4F4F;
    border: 1px solid #4F4F4F;
    padding: 10px 14px;
    margin: 0px 6px 0px 20px;
    cursor: pointer;
}

.task .task__partialData .task__users .task__users-btn .task__users-btn-delete{
    font-size: 14px;
    color: #4F4F4F;
    font-family: 'Open Sans', sans-serif;
    background: #E0E0E0;
    border: 1px solid #E0E0E0;
    padding: 10px 14px;
    margin: 0px 0px 0px 6px;
    cursor: pointer;
}

.task .task__partialData .task__errText{
    font-size: 18px;
    font-family: 'Open Sans', sans-serif;
    padding-top: 10px;
}
.task .task__details{
    margin-left: 40px;
}

.taskShow{
    display: block;
}

.task .task__details h3{
    font-size: 36px;
    font-family: 'Open Sans', sans-serif;
    margin-bottom: 0;
}

.task .task__details .task__details-identifier{
    font-size: 14px;
    font-family: 'Open Sans', sans-serif;
    color: #4F4F4F;
    padding-top: 4px;
}

.task .task__details .task__details-company h3,
.task .task__details .task__details-website h3,
.task .task__details .task__details-phone h3,
.task .task__details .task__details-address h3{
    font-size: 24px;
    font-family: 'Open Sans', sans-serif;
    margin-bottom: 10px;
}

.task .task__details .task__details-address p{
    font-size: 18px;
    font-family: 'Open Sans', sans-serif;
    margin: 0;
}

.task .task__details .task__details-address span{
    font-size: 14px;
    color: #000;
    font-family: 'Open Sans', sans-serif;
}

.task .task__details .task__details-address span p{
    font-size: 14px;
    color: #4F4F4F;
    font-family: 'Open Sans', sans-serif;
    display: inline;
    
}

.task .task__details .task__details-company p,
.task .task__details .task__details-website p,
.task .task__details .task__details-phone p{
    font-size: 18px;
    font-family: 'Open Sans', sans-serif;
    margin: 0px 0px 6px 0px;
}

.task .task__details .task__details-company span{
    font-size: 14px;
    font-family: 'Open Sans', sans-serif;
    color: #4F4F4F;
}
</style>