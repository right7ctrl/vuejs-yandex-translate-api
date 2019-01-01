<template>
  <div id="app">
    <div class="container contact-form">
      <div class="contact-image">
        <img src="https://image.ibb.co/kUagtU/rocket_contact.png" alt="rocket_contact"/>
      </div>
      <form>
        <h3>Vue.js Translator</h3>
        <div style="margin: 15px 0;" class="row">
          <div class="col-md-6">
            <div style="width: 100%;" class="form-group">
              <select class="form-control" v-model="currentLang">
                <option value="" selected>Current Language</option>
                <option v-for="lang in langList" :value="lang[0]">{{lang[1]}}</option>
              </select>
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <select class="form-control" v-model="targetLang">
                <option value="" selected>Target Language</option>
                <option v-for="lang in langList" :value="lang[0]">{{lang[1]}}</option>
              </select>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-6">
            <div class="form-group">
              <textarea name="txtMsg" v-model="textToTranslate" v-on:keypress="translate" class="form-control"
                        placeholder="Type something and hit enter" style="width: 100%; height: 150px;"></textarea>
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <textarea v-if="status" name="txtMsg" class="form-control" placeholder="Translated text will be here" readonly
                        style="width: 100%; height: 150px;">
                {{translatedText}}
              </textarea>
            </div>
          </div>
        </div>
      </form>
      <div class="row" style="text-align: center; padding-bottom: 15px;">
        <div class="col-md-12">
          <button v-if="textToTranslate" @click="translate" class="btn btn-primary col-md-6">Translate</button>
          <button v-if="textToTranslate" @click="swapLanguages" style="float:left;" class="btn btn-warning col-md-6">Swap Languages</button>
        </div>

      </div>



    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        targetLang: '',
        currentLang: '',
        textToTranslate: '',
        status: true,
        translatedText: '',
        langList: {
          tr: ['tr', "Turkish"],
          en: ['en', "English"],
          fr: ['fr', "French"],
          es: ['es', "Spanish"],
          de: ['de', "German"],
          ru: ['ru', "Russian"],
        }
      }
    },


    methods: {
      translate() {
        if (this.currentLang && this.targetLang && this.textToTranslate) {
          fetch('https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20190101T161348Z.7964b117864ab14a.588a2858757de23eb135dc80336998ed959d02a9&text=' + this.textToTranslate + '&lang=' + this.currentLang + '-' + this.targetLang,
            {method: 'GET', headers: {Accept: 'text/json'}})
            .then((res) => {
              return res.json()
            })
            .then((res) => {
              if (res['code'] === 200) {
                this.status = true;
                this.translatedText = res['text'][0];
                console.log('ok');
              } else {
                this.status = false;
                console.log("something went wrong")
              }
            });
        }
      },
      swapLanguages(){
        let current = this.currentLang;
        this.currentLang = this.targetLang;
        this.targetLang = current;

        let translated = this.translatedText;
        this.translatedText = this.textToTranslate;
        this.textToTranslate = translated;
      }
    },

    created() {

      console.log(langList);


    }
  }
</script>

<style lang="scss">
  @import '~bootstrap/scss/bootstrap.scss';

  body {
    background: -webkit-linear-gradient(left, #0072ff, #00c6ff);
  }

  .contact-form {
    background: #fff;
    margin-top: 10%;
    margin-bottom: 5%;
    width: 70%;
  }

  .contact-form .form-control {
    border-radius: 1rem;
  }

  .contact-image {
    text-align: center;
  }

  .contact-image img {
    border-radius: 6rem;
    width: 11%;
    margin-top: -3%;
    transform: rotate(29deg);
  }

  .contact-form form {
    padding: 14%;
  }

  .contact-form form .row {
    margin-bottom: -7%;
  }

  .contact-form h3 {
    margin-bottom: 8%;
    margin-top: -10%;
    text-align: center;
    color: #0062cc;
  }

  .contact-form .btnContact {
    width: 50%;
    border: none;
    border-radius: 1rem;
    padding: 1.5%;
    background: #dc3545;
    font-weight: 600;
    color: #fff;
    cursor: pointer;
  }

  .btnContactSubmit {
    width: 50%;
    border-radius: 1rem;
    padding: 1.5%;
    color: #fff;
    background-color: #0062cc;
    border: none;
    cursor: pointer;
  }
</style>
