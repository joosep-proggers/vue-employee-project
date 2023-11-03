<script>
  export default {
    name: 'DetailViewModal',
    props: ['employeeData'],
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
            {{ employeeData.fname}} {{ employeeData.lname}}
            <div class="employee-position">
              {{ employeeData.position }}
            </div>
          </slot>
          <button type="button" class="btn-close" @click="close" aria-label="Close modal">x</button>
        </header>

        <section class="modal-body" id="modalDescription">
          <slot name="body">
            {{ employeeData }}
          </slot>
        </section>

        <footer class="modal-footer">
          <slot name="footer">
            This is the default footer!
          </slot>
        </footer>
      </div>
    </div>
  </transition>
</template>

<style>
  .employee-position {
    padding-top: 2vh;
    font-size: 2vh;
  }

  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10vw;
  } 

  .modal {
    background: #FFFFFF;
    border-style: solid;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header {
    padding: 4vh;
    display: flex;
    font-size: 3vh;
  }

  .modal-footer {
    padding: 5vw;
    display: flex;
    font-size: 2vh;
  }

  .modal-header {
    position: relative;
    border-bottom: 1px solid #eeeeee;
    justify-content: space-between;
    position: relative;
    flex-direction: column;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    flex-direction: column;
  }

  .modal-body {

    padding: 5vw 5vh;
  }

  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 5vh;
    padding: 1vh;
    margin-right: 1vh;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .modal-fade-enter,
  .modal-fade-leave-to {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .2s ease;
  }
</style>