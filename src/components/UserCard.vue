<!-- eslint-disable prettier/prettier -->
<template>
  <div class="utente-card">
    <p><strong>Nome:</strong> {{ utente.nome }}</p>
    <p><strong>Cognome:</strong> {{ utente.cognome }}</p>
    <p><strong>Data di nascita:</strong> {{ formatDate(utente.data_nascita) }}</p>
    <p><strong>QR Code:</strong> {{ utente.qr_code }}</p>

    <qrcode-vue
      :value="utente.qr_code"
      :size="100"
      level="M"
      style="margin: 10px 0"
    />

    <!-- Spese fatte con contatore -->
    <p>
      <strong>Spese fatte:</strong>
      <span>{{ localSpese }}</span>
    </p>

    <div class="buttons">
      <button @click="incrementSpese" class="btn btn-sm btn-secondary">+1</button>
      <button @click="saveSpese" class="btn btn-sm btn-primary">Salva</button>
    </div>
  </div>
</template>
<!-- eslint-disable prettier/prettier -->

<script>
import QrcodeVue from "qrcode.vue";

export default {
  name: "UserCard",
  components: { QrcodeVue },
  props: {
    utente: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      localSpese: this.utente.spese_fatte ? Number(this.utente.spese_fatte) : 0,
    };
  },
methods: {
  incrementSpese() {
    this.localSpese += 1;
  },
  async saveSpese() {
    await this.$store.dispatch("updateUser", {
      qr_code: this.utente.qr_code,
      updatedFields: { spese_fatte: this.localSpese },
      index: this.index,
    });
    alert("Spese aggiornate correttamente!");
  },
  formatDate(dateStr) {
    if (!dateStr) return "-"; // se non è presente
    const d = new Date(dateStr);
    const day = String(d.getDate()).padStart(2, "0");
    const month = String(d.getMonth() + 1).padStart(2, "0");
    const year = String(d.getFullYear()).slice(-4); // ultimi due numeri dell’anno
    return `${day}/${month}/${year}`;
  },
},
};
</script>
<!-- eslint-disable prettier/prettier -->

<style scoped>
.utente-card {
  margin-bottom: 2rem;
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  max-width: 300px;
  background-color: #fafafa;
}

.buttons {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.5rem;
}
</style>
