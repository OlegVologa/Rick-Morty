<template>
  <div class="footer d-flex flex-wrap text-center justify-content-center py-4">
    <p class="footer-text col-12 col-sm-4">© Vologa Oleg 2024</p>
    <p class="footer-phone col-12 col-sm-4">
      <img class="footer-icon" src="../assets/whatsapp.svg" alt="telephon-icon" />
      <a href="tel:+79518229344" class="phone d-inline-flex ps-3">+7(951)-822-93-44</a>
    </p>
  </div>
</template>

<style lang="scss" scoped>
.footer {
  background: rgb(32, 35, 41);
  color: white;
  font-size: 18px;
}
.phone {
  text-decoration: none;
  color: inherit;
}

.footer-icon {
  position: relative;
  top: -2px;
}
.phone {
  position: relative;
  transition: all 0.5s ease-in;
}

.phone:hover {
  transform: scale(1.1);
}
</style>
