<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <!-- BOUTTON DE MENU -->
        <ion-buttons slot="start">
          <ion-back-button @click="this.router.back()"></ion-back-button>
        </ion-buttons>
        <ion-title class="labelColor">Recette</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div id="mainContainer">
        <!-- MARK: IMAGE -->
        <div id="imageContainer">
          <ion-img class="rounded" :src="this.maListe[0].image"></ion-img>
        </div>
        <!--  MARK: TITRE RECETTE-->
        <br />
        <br />
        <ion-label class="titleOne">{{ this.maListe[0].nom }}</ion-label>
        <!--  MARK: DETAILS RECETTE-->

        <div id="detailContainer">
          <ion-label
            >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Origine:&nbsp;{{ this.maListe[0].origine }} &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Catégorie:&nbsp;{{
              this.maListe[0].categorie
            }}</ion-label
          >
        </div>

        <!-- MARK: INGREDIENTS RECETTE -->
        <br />
        <br />
        <div id="ingredientContainer">
          <ion-label class="titleOne"><u>Ingrédients</u></ion-label>
          <br />
          <ul>
            <li v-for="index in this.nbreItem" :key="index">
              <span>{{ this.maListe[0].measure[index - 1] }}</span>
              -
              <span>{{ this.maListe[0].ingredients[index - 1] }}</span>
            </li>
          </ul>
        </div>

        <!-- MARK: INSTRUCTIONS RECETTE -->
        <div id="instructionContainer">
          <p v-for="index in this.nbreInstruction" :key="index">{{ this.maListe[0].steps[index] }}</p>
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, IonButtons, IonBackButton, IonLabel, IonImg } from "@ionic/vue";
import { defineComponent } from "vue";
import { useRoute, useRouter } from "vue-router";
import { loadingController } from "@ionic/vue";

export default defineComponent({
  name: "HomePage",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonButtons,
    IonBackButton,
    IonImg,
    IonLabel,
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const id = route.params.id;
    return { route, router, id };
  },
  data() {
    var emptyArray: string[] = [];

    return {
      maListe: [
        {
          id: "",
          nom: "",
          origine: "",
          categorie: "",
          image: "",
          steps: emptyArray,
          ingredients: emptyArray,
          measure: emptyArray,
        },
      ],
      nbreItem: 0,
      nbreInstruction: 0,
    };
  },

  ionViewDidEnter() {
    this.getJSON();
  },

  methods: {
    async getJSON() {
      const loading = await loadingController.create({
        message: "Attendre SVP...",
      });

      await loading.present();

      let url = `https://www.themealdb.com/api/json/v1/1/lookup.php?i=` + this.id;
      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          let instructionSplit = data.meals[0].strInstructions.split(".");

          var countInstruction = 0;
          for (var i = 0; i < instructionSplit.length - 1; i++) {
            instructionSplit[i] = i + "-" + instructionSplit[i] + ".";
            countInstruction++;
          }

          this.nbreInstruction = countInstruction;

          this.maListe[0].id = data.meals[0].idMeal;
          this.maListe[0].nom = data.meals[0].strMeal;
          this.maListe[0].origine = data.meals[0].strArea;
          this.maListe[0].categorie = data.meals[0].strCategory;
          this.maListe[0].image = data.meals[0].strMealThumb;
          this.maListe[0].steps = instructionSplit;
          this.maListe[0].ingredients = [
            data.meals[0].strIngredient1,
            data.meals[0].strIngredient2,
            data.meals[0].strIngredient3,
            data.meals[0].strIngredient4,
            data.meals[0].strIngredient5,
            data.meals[0].strIngredient6,
            data.meals[0].strIngredient7,
            data.meals[0].strIngredient8,
            data.meals[0].strIngredient9,
            data.meals[0].strIngredient10,
            data.meals[0].strIngredient11,
            data.meals[0].strIngredient12,
            data.meals[0].strIngredient13,
            data.meals[0].strIngredient14,
            data.meals[0].strIngredient15,
            data.meals[0].strIngredient16,
            data.meals[0].strIngredient17,
            data.meals[0].strIngredient18,
            data.meals[0].strIngredient19,
            data.meals[0].strIngredient20,
          ];
          this.maListe[0].measure = [
            data.meals[0].strMeasure1,
            data.meals[0].strMeasure2,
            data.meals[0].strMeasure3,
            data.meals[0].strMeasure4,
            data.meals[0].strMeasure5,
            data.meals[0].strMeasure6,
            data.meals[0].strMeasure7,
            data.meals[0].strMeasure8,
            data.meals[0].strMeasure9,
            data.meals[0].strMeasure10,
            data.meals[0].strMeasure11,
            data.meals[0].strMeasure12,
            data.meals[0].strMeasure13,
            data.meals[0].strMeasure14,
            data.meals[0].strMeasure15,
            data.meals[0].strMeasure16,
            data.meals[0].strMeasure17,
            data.meals[0].strMeasure18,
            data.meals[0].strMeasure19,
            data.meals[0].strMeasure20,
          ];

          var countIng = 0;

          this.maListe[0].measure.forEach((element) => {
            if (element.length > 1) {
              countIng++;
            }
          });

          this.nbreItem = countIng;

          loading.dismiss();
        });
    },
  },
});
</script>
<style scoped>
/* Added CSS*/
#mainContainer {
  margin: auto;
  margin-top: 40px;
  text-align: center;
}

@media screen and (min-width: 0px) {
  #imageContainer {
    margin: auto;
    border-radius: 75px;
    overflow: hidden;
    width: 50%;
  }

  #detailContainer {
    color: orange;
    margin: auto;
    margin-top: 40px;
    width: 200px;
    text-align: center;
    border: 2px solid turquoise;
    border-radius: 25px;
  }
}

@media screen and (min-width: 1000px) {
  #imageContainer {
    margin: auto;
    border-radius: 75px;
    overflow: hidden;
    width: 351px;
  }
}

#ingredientContainer {
  margin: auto;
  width: 100%;
  text-align: center;
}

#instructionContainer {
  color: orange;
  margin: auto;
  width: 50%;
  margin-top: 40px;
  text-align: justify;
}
.center {
  margin: auto;
  width: 50%;
}
.titleOne {
  font-size: xx-large;
  color: turquoise;
  font-family: "Times New Roman", Times, serif;
}

.bordered {
  text-align: center;
  border: 2px solid red;
}
ul {
  list-style-type: none;
  padding: 0;
}
ul li {
  list-style-type: none;
  width: 100%;
  position: relative;
}
ul li span {
  color: orange;
  position: absolute;
  right: 0;
  left: 50%;
  text-align: left;
  padding-left: 5px;
  display: inline-block;
}
ul li span:first-child {
  color: orange;
  position: absolute;
  left: 0;
  right: 50%;
  text-align: right;
  padding-right: 5px;
  display: inline-block;
}

.labelColor {
  color: turquoise;
}
</style>
