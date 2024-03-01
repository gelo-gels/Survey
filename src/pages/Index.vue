<template>
  <q-page class="flex flex-center bg-black">

<form @submit.prevent="handleSubmit" name="contact" method="POST" data-netlify="true">
	<q-input required type="text" name="message" v-model="form.message" borderless label="message" style="visibility: hidden"/>
    <div class="container_form">

    	<div class="text-bold font-poppins-bold font_sizing q-mb-lg" v-if="name_submits == true">
    		 SURVEY
    	</div>
	    	<div class="font-poppins-bold" v-if="name_submits == true">
	    		

	    		<q-input required type="text" name="name" v-model="form.name" borderless label="Please input your name" style="box-shadow: 0px 0px 10px 2px; border-radius: 10px; padding: 0px 10px" dense color="black" class="q-mb-md" />
	    		<div class="q-mb-md text-red" v-if="error_message != ''">
	    			Please type your name!
	    		</div>
		    </div>
		    <div class="font-poppins-bold" v-if="name_submits == true">
		    	<q-btn no-caps label="Submit" class="btns" @click="click_submit()"/>
		    </div>



    	<div class="text-bold font-poppins-bold font_sizing q-mb-xl" v-if="survey == true">
    		 DO YOU LIKE ME?
    	</div>
    	<div class="row font-poppins-bold">
	    	<div class="col-6" v-if="survey == true">
	    		<q-btn no-caps label="YES" type="submit" class="btns" @click="click_oo()"  />
	    	</div>
	    	<div class="col-6 text-right" :class="survey != true ? 'visibility: hidden' : 'visibility: visible'">
	    		<q-btn no-caps label="NO!!" class="btns"  id="btn_test" />
	    	</div>

	    	<!-- hidden -->
	    	<!-- <div class="col-6 text-right"> 
	    		<q-btn no-caps label="Wala" class="btns"  id="btn_test" />
	    	</div> -->

	    </div>



	    <div v-if="word_reveal == true" class="text-bold font-poppins-bold font_sizing_reveal">
    		I LIKE YOU TOO!!!!
    	</div>
    	<div v-if="word_reveal == true" class="text-bold font-poppins-bold font_sizing_reveal">
    		MWAAAAAAAA!
    	</div>
    </div>
</form>



  </q-page>
</template>

<script>
import { ref } from 'vue';

export default({
  name: 'PageIndex',

  setup() {
  	return {
  		survey: ref(false),
  		word_reveal: ref(false),
  		name_submits: ref(true),
  	}
  },

  data() {
  	return {
  		error_message: '',

  		form: {
  			name: '',
  			message: 'just click "Oo"'
  		}
  	}
  },

  methods: {
  	async click_oo() {
  		setTimeout(()=>{
				this.word_reveal = true
		  		this.survey = false
		  		console.log(this.form.name)
			},500);
  	},

  	async click_submit() {
  		if (this.form.name === '' || this.form.name === null || this.form.name.value === 0){
            this.error_message = 'please type your name!'
        }
        else {
        	this.name_submits = false
  			this.survey = true
        }
  		

		console.log(this.form.name)
  	},

  	encode(data) {
		  return Object.keys(data)
		  .map(key => `${encodeURIComponent(key)}=${encodeURIComponent(data[key])}`)
		  .join('&')
	  },
  		handleSubmit() {
			fetch('/', {
				method: 'post',
				headers: {
					'Content-type': 'application/x-www-form-urlencoded'
				},
				body: this.encode({
					'form-name': 'list_of_clicks',
					...this.form
				})
			})
			.then(() => console.log('successfully sent'))
			.catch(e => console.error(e))

			this.dialog = true
			 setTimeout(()=>{
				this.dialog = false
			},3000);
	  },
  },

  mounted() {
	//sets a random absolute position to a html element; receives the html element
	function moveElmRand(elm){
	 elm.style.position ='absolute';
	 elm.style.top = Math.floor(Math.random()*90+5)+'%';
	 elm.style.left = Math.floor(Math.random()*90+5)+'%';
	}

	//get the #btn_test
	var btn_test = document.querySelector('#btn_test');

	//register to call moveElmRand() on mouseenter event to #btn_test
	btn_test.addEventListener('mouseenter', function(e){ moveElmRand(e.target);});

	//register click to #btn_test
	btn_test.addEventListener('click', function(e){ alert('Wao namern bilis ng kamay.');});

  }
})
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&display=swap');


.font-poppins-reg {
	font-family: 'Poppins', sans-serif;
}
.font-poppins-bold {
	font-family: 'Poppins', sans-serif;
	font-weight: 800
}
	.container_form {
		background: pink;
		padding: 50px;
		border-radius: 20px;
		margin: 0px 20px
	}

	.font_sizing {
		font-size: 50px
	}
	.font_sizing_reveal {
		font-size: 50px
	}
	.btns {
		color: pink; 
		background: black; 
		font-size: 20px; 
		padding: 0px 20px 0px 20px;
		border-radius: 10px;
		box-shadow: 0px 0px 20px 5px pink;
	}


	@media (max-width: 646px) {
		.font_sizing_reveal {
			font-size: 30px
		}
	}
	@media (max-width: 520px) {
		.container_form {
			padding: 20px;
			margin: 0px 20px
		}
	}
</style>
