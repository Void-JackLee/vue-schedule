<template>
  <!--Saturday-->
  <div id="app">
    <Schedule
        :time-ground="['09:00', '21:00']"
        :week-ground="['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']"
        :color="[
        '#2B2E4A',
        '#521262',
        '#903749',
        '#53354A',
        '#40514E',
        '#537780',
      ]"
        :task-detail="[
        [
          {
            dateStart: '09:30',
            dateEnd: '10:30',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
            detail: 'Metting (German: Mettingen) is a commune in the Moselle department in Grand Est in north-eastern France.'
          },
          {
            dateStart: '11:30',
            dateEnd: '13:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
            detail: 'Metting (German: Mettingen) is a commune in the Moselle department in Grand Est in north-eastern France.'
          },

        ],
        [
          {
            dateStart: '10:30',
            dateEnd: '12:00',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          },
          {
            dateStart: '12:30',
            dateEnd: '14:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          }

        ],
        [
          {
            dateStart: '12:30',
            dateEnd: '13:30',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          },
          {
            dateStart: '15:30',
            dateEnd: '16:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          }

        ],
        [
          {
            dateStart: '09:50',
            dateEnd: '10:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          },
          {
            dateStart: '11:30',
            dateEnd: '13:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          }

        ],
        [
          {
            dateStart: '12:30',
            dateEnd: '13:30',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          },
          {
            dateStart: '14:30',
            dateEnd: '15:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          }

        ],
        [
          {
            dateStart: '14:30',
            dateEnd: '15:30',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          },
          {
            dateStart: '17:30',
            dateEnd: '19:50',
            title:'Metting',html: `<h3 style='color: #E0E7E9;margin: 1rem 0 0 1rem;'>Metting</h3>`,
          }

        ]
      ]"
    style="width: 80%" @click="showModal">

    </Schedule>
    <modal :show="show" :show-modal-detail="showModalDetail"></modal>
  </div>
</template>

<script>
import Schedule from "./components/Schedule";
import Modal from "./components/Modal";

export default {
  name: 'app',
  components: {
    Modal,
    Schedule
  },
  data() {
    return {
      show: false,
      showModalDetail: {},
    }
  },
  methods: {
    showModal(obj) {
      this.show = !this.show;
      this.showModalDetail=obj;
    }
  }
}
</script>

<style>

</style>
