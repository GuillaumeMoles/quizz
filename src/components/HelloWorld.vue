<template>
    <div class="card">
        <div class="card-header">Bienvenu à question pour un stagiaire !</div>
        <div class="card-body">
            <div>Vous aller devoir répondre à quelques questions mais avant</div>
            <div>Sur quel thème souhaitez vous être intérogé</div>
            <label>
                <select v-model="selectedThemeId" v-on:change="themeAvailable = true">
                    <option v-for="item in listCriteria" :key="item.id" v-bind:value="item.id">{{ item.name }}</option>
                </select>
            </label>
            <div v-if="!themeAvailable" class="text-danger">Vous devez renseigner le thème des questions</div>
            <div>{{ selectedThemeId }}</div>
            <div>Et combien de questions souhaitez vous ?</div>
            <label>
                <input placeholder="" v-model="nbQuestions" v-on:change="nbAvailable = true">
            </label>
            <div v-if="!nbAvailable" class="text-danger">Vous devez renseigner le nombre de questions</div>
            <div>{{ nbQuestions }}</div>
            <button v-on:click="getQuestions" class="mt-3">MES QUESTIONS</button>
        </div>
        <div></div>
    </div>
</template>

<script>

import axios from "axios";

export default {
    name: 'HelloWorld',
    data() {
        return {
            nbQuestions: 0,
            listCriteria: [],
            selectedThemeId: null,
            questionsDatas: null,
            nbAvailable: true,
            themeAvailable: true
        }
    },
    methods: {
        getQuestions() {
            if (this.nbQuestions > 0 && this.selectedThemeId) {
                console.log(`https://opentdb.com/api.php?amount=${this.nbQuestions}&category=${this.selectedThemeId}`);
                axios.get(`https://opentdb.com/api.php?amount=${this.nbQuestions}&category=${this.selectedThemeId}`)
                    .then(res => {
                        console.log(res);
                        if (res.status === 200) {
                            console.log(res.data);
                            this.questionsDatas = res.data.results;
                        }
                    });
                console.log(this.questionsDatas);
            }
            else {
                if (!(this.nbQuestions > 0)) this.nbAvailable = false;
                if (!this.selectedThemeId) this.themeAvailable = false;
            }

        }
    },
    mounted() {
        axios.get('https://opentdb.com/api_category.php')
            .then(res => {
                console.log(res);
                if (res.status === 200) {
                    console.log("RESPONSE");
                    console.log(res.data.trivia_categories);
                    this.listCriteria = res.data.trivia_categories;
                    this.selectedTheme = res.data.trivia_categories[0].name;
                }
            });
    },
    props: {
        msg: String
    }
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
