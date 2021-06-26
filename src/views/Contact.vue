<template>
  <div>
    <h1>Kontakt</h1>

    <br />

    <form>

      <v-text-field label="Unesite email.." filled></v-text-field>

      <v-text-field
          label="Unesite naslov.."
          hint="Ovo polje je obavezno"
          filled
      ></v-text-field>

      <v-textarea
          label="Unesite opis.."
          hint="Ovo polje je obavezno"
          filled
      ></v-textarea>

      <v-file-input accept="image/*" label="Priložite sliku" filled></v-file-input>

    </form>
  </div>
</template>

<script>
export default {
  name: "Contact",
};
</script>