<template>
    <div class='formgn'>
    <h1> Cadastro de Planejamento </h1>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="input-group-2" label="Nome do Planejamento:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.nomeDoPlan"
          placeholder="Insira nome"
        ></b-form-input>
      </b-form-group>

       <b-form-group id="input-group-3" label="Plano de Ação:" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.planoDeAcao"
          placeholder="Insira o plano de ação"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-5" label="Plano de Ação:" label-for="input-5">
        <b-form-input
          id="input-5"
          v-model="form.acao1"
          placeholder="Insira a ação"
        ></b-form-input>
        <b-form-input
          id="input-6"
          v-model="form.data1"
          placeholder="Insira a data"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-6" label="Plano de Ação:" label-for="input-6">
        <b-form-input
          id="input-7"
          v-model="form.acao2"
          placeholder="Insira a ação"
        ></b-form-input>
        <b-form-input
          id="input-8"
          v-model="form.data2"
          placeholder="Insira a data"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-7" label="Plano de Ação:" label-for="input-7">
        <b-form-input
          id="input-9"
          v-model="form.acao3"
          placeholder="Insira a ação"
        ></b-form-input>
        <b-form-input
          id="input-10"
          v-model="form.data3"
          placeholder="Insira a data"
        ></b-form-input>
      </b-form-group>
      <b-button type="submit" variant="primary">Salvar</b-button>
      <b-button type="reset" variant="danger">Limpar</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      form: {
        nomeDoPlan: '',
        planoDeAcao: '',
        acao1: '',
        data1: '',
        acao2: '',
        data2: '',
        acao3: '',
        data3: ''
      },
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      console.log('OnSubmit')
      console.log('token' + this.$root.$token)
      if (this.$root.$token !== '' && this.$root.$isValidToken) {
        // let formData = this.gatherFormData()

        let headers = {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + this.$root.$token
        }

        // for (var value of formData.entries()) {
        //   console.log(value)
        // }

        // var object = {}
        // for (var pair of formData.entries()) {
        //   object[pair[0]] = pair[1]
        // }
        // formData.forEach((value, key) => object[key] = value)
        // var json = JSON.stringify(object)
        let object = this.gatherFormData()
        let json = JSON.stringify(object)
        console.log(json)

        this.$http.post('http://sigoapp.southcentralus.azurecontainer.io:3000/sigo/gestaonormas/planeja', json, {headers})
          .then(res => {
            alert('Planejamento cadastrado com sucesso')
          })
          .catch(res => {
            if (res.status === 404 || res.status === 401) {
              this.$root.$isValidToken = false
              console.log('token invalidado')
              alert('Sessão expirada, favor refazer o login')
              this.$router.push('/sigo/autenticacao')
            } else {
              alert('Planejamento não cadastrado, verifique os dados')
            }
          })
      }
    },
    gatherFormData () {
      // const data = new FormData()
      // data.append('nomeDoPlan', this.form.nomeDoPlan)
      // data.append('planoDeAcao', this.form.planoDeAcao)
      // const etapa = new FormData()
      // etapa.append(this.form.acao1, this.form.data1)
      // etapa.append(this.form.acao2, this.form.data2)
      // etapa.append(this.form.acao3, this.form.data3)
      // var object = {}
      // for (var pair of etapa.entries()) {
      //   object[pair[0]] = pair[1]
      // }
      // etapa.forEach((value, key) => object[key] = value)
      // var json = JSON.stringify(object)
      // data.append('etapaData', object)
      const data = {}
      data['nomeDoPlan'] = this.form.nomeDoPlan
      data['planoDeAcao'] = this.form.planoDeAcao
      let etapaData = {}
      etapaData[this.form.acao1] = this.form.data1
      etapaData[this.form.acao2] = this.form.data2
      etapaData[this.form.acao3] = this.form.data3
      data['etapaData'] = etapaData
      return data
    },
    onReset (evt) {
      evt.preventDefault()
      // Reset our form values
      this.form.name = ''
      this.form.obs = ''
      this.form.normafile = ''
      // Trick to reset/clear native browser form validation state
      this.show = false
      this.$nextTick(() => {
        this.show = true
      })
    }
  }
}
</script>

<style>
.formgn{
    width: 50%;
    margin: auto;
}

.mt-3{
    margin:50px 50px;
}
</style>
