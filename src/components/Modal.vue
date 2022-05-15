<script>
  export default {
    name: 'Modal',
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>

<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal" role="dialog" aria-labelledby="modalTitle" aria-describedby="modalDescription">
        <header class="modal-header" id="modalTitle">
          <slot name="header">
            This is the default tile!
          </slot>
        </header>

        <section class="modal-body" id="modalDescription">
          <slot name="body">
            This is the default body!
          </slot>
        </section>

        <footer class="modal-footer">
          <button type="button_accept" class="btn-green" @click="close" aria-label="Close modal"> Aceptar </button>
          <button type="button_deny" class="btn-red" @click="close" aria-label="Close modal"> Cancelar </button>
        </footer>
      </div>
    </div>
  </transition>
</template>