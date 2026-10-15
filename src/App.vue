<script setup>
import { ref } from 'vue'

let poids = ref('')
let taille = ref('')
let afficherResultat = ref(false)

function imc() {
  let kg = Number(poids.value)
  let metres = Number(taille.value) / 100
  if (kg <= 0 || metres <= 0) {
    return null
  }
  return kg / (metres * metres)
}

function categorie() {
  let valeurImc = imc()
  if (valeurImc === null) {
    return ''
  }
  if (valeurImc < 18.5) {
    return 'Insuffisance pondérale'
  }
  if (valeurImc < 25) {
    return 'Corpulence normale'
  }
  if (valeurImc < 30) {
    return 'Surpoids'
  }
  return 'Obésité'
}

function explication() {
  let valeurImc = imc()
  if (valeurImc === null) {
    return ''
  }
  if (valeurImc < 18.5) {
    return 'Votre IMC est inférieur à la plage habituelle.'
  }
  if (valeurImc < 25) {
    return 'Votre IMC se situe dans la plage habituelle.'
  }
  if (valeurImc < 30) {
    return 'Votre IMC se situe dans la plage du surpoids.'
  }
  return "Votre IMC se situe dans la plage de l'obésité."
}

function calculer() {
  afficherResultat.value = imc() !== null
}

function reinitialiser() {
  poids.value = ''
  taille.value = ''
  afficherResultat.value = false
}
</script>

<template>
  <main>
    <section>
      <h1>Calculateur d'IMC</h1>
      <p>Saisissez votre poids et votre taille pour calculer votre indice de masse corporelle.</p>
      
      <form @submit.prevent="calculer">
        <div>
          <label>
            Poids (en kg)
            <input
              v-model="poids"
              type="number"
              inputmode="decimal"
              min="1"
              max="500"
              step="any"
              required
              placeholder="Ex. 70"
              @input="afficherResultat = false"
            />
          </label>
        </div>
        
        <div>
          <label>
            Taille (en cm)
            <input
              v-model="taille"
              type="number"
              inputmode="decimal"
              min="50"
              max="250"
              step="any"
              required
              placeholder="Ex. 175"
              @input="afficherResultat = false"
            />
          </label>
        </div>

        <div>
          <button type="submit">Calculer mon IMC</button>
          <button type="button" @click="reinitialiser">Effacer</button>
        </div>
      </form>

      <div v-if="afficherResultat">
        <h2>Votre IMC</h2>
        <p><strong>{{ imc() }}</strong></p>
        <p><strong>{{ categorie() }}</strong></p>
        <p>{{ explication() }}</p>
      </div>

      <p>IMC = poids (kg) / taille (m)². Cet indicateur concerne les adultes et ne remplace pas un avis médical.</p>
    </section>
  </main>
</template>