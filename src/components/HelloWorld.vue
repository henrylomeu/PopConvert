<template>
  <div>
    <button @click="showPopup(1)">Abrir Pop-up 1</button>
    <button @click="showPopup(2)">Abrir Pop-up 2</button>

    <div v-if="currentPopup" class="popup-overlay">
      <div class="popup">
        <button class="close-button" @click="closePopup">&times;</button>
        <h1>{{ currentPopup.title }}</h1>
        <h2>{{ currentPopup.subtitle }}</h2>
        <img v-if="currentPopup.image" :src="currentPopup.image" :alt="currentPopup.title" class="popup-image"/>

        <video v-if="currentPopup.videoUrl" :src="currentPopup.videoUrl" controls></video>

        <form @submit.prevent="submitForm">
          <div v-for="field in currentPopup.formFields" :key="field.id" class="form-field">
            <label :for="field.id">{{ field.label }}</label>
            <div v-if="field.model === 'email'" class="field-wrapper">
              <input :type="field.type" :id="field.id" v-model="form[field.model]" :required="field.required" />
            </div>
            <div v-else-if="field.model === 'name'" class="field-wrapper">
              <input :type="field.type" :id="field.id" v-model="form[field.model]" :required="field.required" />
            </div>
            <div v-else-if="field.model === 'phone'" class="field-wrapper">
              <input :type="field.type" :id="field.id" v-model="form[field.model]" :required="field.required" class="phone-field" />
            </div>
            <div v-else-if="field.model === 'gender'" class="field-wrapper">
              <select :id="field.id" v-model="form[field.model]" :required="field.required">
                <option value="" disabled selected>Selecione</option>
                <option value="masculino">Masculino</option>
                <option value="feminino">Feminino</option>
              </select>
            </div>
            <div v-else>
              <input :type="field.type" :id="field.id" v-model="form[field.model]" :required="field.required" />
            </div>
          </div>

          <button type="submit">Enviar</button>
        </form>
      </div>
    </div>
  </div>
  <div v-if="showSuccessModal" class="success-modal-overlay">
  <div class="success-modal">
    <h1>Cadastro feito com sucesso!</h1>
    <button class="close-button2" @click="closeSuccessModal">&times;</button>
    </div>
  </div>



</template>



<script>
import popupConfig from '../popupConfig.json';

export default {
  data() {
    return {
      currentPopup: null,
      form: {},
      showSuccessModal: false
    };
  },
  methods: {
    showPopup(popupIndex) {
      this.currentPopup = popupConfig[popupIndex - 1];
      this.resetForm();
    },
    closePopup() {
      this.currentPopup = null;
    },
    submitForm() {
      console.log('Dados do Pop-up:', this.form);
      this.resetForm();
      this.currentPopup = null;
    },
    resetForm() {
      this.form = {};
      this.currentPopup.formFields.forEach(field => {
        this.form[field.model] = '';
      });
    },
    getFieldClass(field) {
      if (field.model === 'phone') {
        return 'phone-field';
      }
      return '';
    },
    submitForm() {
      console.log('Dados do Pop-up:', this.form);
      this.resetForm();
      this.currentPopup = null;
      this.showSuccessModal = true; 
    },
    closeSuccessModal() {
    this.showSuccessModal = false;
  },
  },
  computed: {
    popupConfig() {
      return popupConfig.map(popup => {
        if (popup.image === 'niquel.jpg') {
          popup.image = niquelImage;
        }
        return popup;
      });
    }
  }
};
</script>

<style>
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
}

.popup {
  position: relative;
  background: black;
  padding: 20px;
  width: 400px;
  max-width: 90%;
  border: 1px solid #ccc;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  border-radius: 15px;
  max-height: 90%;
  overflow-y: auto;
  scrollbar-width: thin; 
  scrollbar-color: #ccc transparent; 
}
.popup::-webkit-scrollbar {
  width: 1px; 
}

.popup::-webkit-scrollbar-thumb {
  background-color: black; 
}
.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 20px;
  background: none;
  border: none;
  color: white;
  cursor: pointer;
}
.popup-image {
  max-width: 100%;
  height: auto;
  margin-bottom: 10px;
}
button {
  margin-right: 10px;
  border-radius: 10px;
}

button:hover {
  border-color: white;
}

.form-field {
  margin-bottom: 10px;
  margin-top: 10px;
}

.checkbox-container {
  display: flex;
  align-items: center;
}

.checkbox-label {
  margin-left: 5px;
}

.field-wrapper input[type="text"],
.field-wrapper input[type="email"],
.field-wrapper input[type="tel"],
select {
  border-radius: 5px;
}

.field-wrapper:hover input,
.field-wrapper:hover select {
  border-color: white;
}

.success-modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  background-color: rgba(0, 0, 0, 0.5);
}

.success-modal {
  position: relative; 
  background: black;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.close-button2 {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 20px;
  background: none;
  border: none;
  color: white; 
  cursor: pointer;
}

@media screen and (max-width: 600px) {
  .popup {
    width: 90%;
    max-width: 100%;
    max-height: 90%;
    overflow-y: auto;
  }
  button{
    margin-top: 15px;
  }
  .success-modal {
    position: relative; 
    background: black;
    padding: 20px;
    border-radius: 10px;
    text-align: center;
    font-size: 15px;
}

.close-button2 {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 20px;
  background: none;
  border: none;
  color: white; 
  cursor: pointer;
}
}
</style>