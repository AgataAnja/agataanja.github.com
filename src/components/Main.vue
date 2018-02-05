<template>
  <div class="small">
    <chart :chart-data="datacollectionOpen"></chart>
    <chart :chart-data="datacollectionOpen"></chart>
  </div>
</template>

<script>
  import Chart from '@/components/Chart.vue'

  export default {
    components: {
      Chart
    },
    data () {
      return {
        datacollectionOpen: null,
        datacollectionClose: null,
        profile: 1
      }
    },
    mounted () {
      this.getData();
    },
    methods: {
      fillData () {
        let openData = {
          label: [],
          average: [],
          last: []
        };
        let closeData = {
          label: [],
          average: [],
          last: []
        };
        this.responseData.forEach(function(el) {
          if (el.Direction === "Open") {
            openData.label.push(el.Position);
            openData.average.push(el.AverageTorque);
            openData.last.push(el.LastTorque);
          } else {
            closeData.label.push(el.Position);
            closeData.average.push(el.AverageTorque);
            closeData.last.push(el.LastTorque);
          }
        });
        this.datacollectionOpen = {
          labels: openData.label,
          datasets: [
            {
              label: 'Average Torque',
              backgroundColor: '#f87979',
              data: openData.average
            }, {
              label: 'Last Torque',
              backgroundColor: '#287979',
              data: openData.last
            }
          ]
        };
        this.datacollectionClose = {
          labels: closeData.label,
          datasets: [
            {
              label: 'Average Torque',
              backgroundColor: '#f87979',
              data: closeData.average
            }, {
              label: 'Last Torque',
              backgroundColor: '#287979',
              data: closeData.last
            }
          ]
        }
      },
      getData () {
        this.$http.get('http://wb-predictivemaintenance-api.prsp7vkew2.eu-central-1.elasticbeanstalk.com/api/TorqueValues')
        .then(response => {
          this.responseData = response.body;
          this.fillData();
      });

      }
    }
  }
</script>

<style>
  .small {
    max-width: 600px;
    height: 400px;
    margin:  150px auto;
  }
</style>
