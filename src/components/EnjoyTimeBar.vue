<template>
  <!-- Happy new Year! -->
  <div id="time-container">
    <div id="time-bar-container">
      <div id="title">Enjoy 2020</div>
      <div id="year-progress">今年的時間進度條: {{year_percentage}}</div>
      <div id="year-graph">
        <div id="year-bar" ref="yearBar"/>
      </div>
      <div id="month-progress">本月的時間進度條: {{month_percentage}}</div>
      <div id="month-graph">
        <div id="month-bar" ref="monthBar"/>
      </div>
      <div id="day-progress">本天的時間進度條: {{day_percentage}}</div>
      <div id="day-graph">
        <div id="day-bar" ref="dayBar"/>
      </div>
      <div id="hour-progress">本小時的時間進度條: {{hour_percentage}}</div>
      <div id="hour-graph">
        <div id="hour-bar" ref="hourBar"/>
      </div>
      <div id="minute-progress">本分鐘的時間進度條: {{minute_percentage}}</div>
      <div id="minute-graph">
        <div id="minute-bar" ref="minuteBar"/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'EnjoyTimeBar',
  data() {
    return {
      def_with: 80,
      year_percentage: '0%',
      month_percentage: '0%',
      day_percentage: '0%',
      hour_percentage: '0%',
      minute_percentage: '0%',
    }
  },
  methods: {
    secondsPerYear(year) {
      if (this.leapYear(year)) {
        return 31622400;
      } else {
        return 31536000;
      }
    },
    leapYear(year) {
      return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0;
    },
    secondsPerMonth(y, m) {
        return new Date(y, m, 0).getDate() * 24 * 3600;
    },
    yearRate(){
      const now = new Date();
      const init_date = new Date(now.getFullYear(), now.getMonth());
      const spy = this.secondsPerYear(now.getFullYear());
      return Math.round((now - init_date) / spy * 100) / 1000;
    },

    monthRate(){
      const now = new Date();
      const init_date = new Date(now.getFullYear(), now.getMonth());
      const spm = this.secondsPerMonth(now.getFullYear(), now.getMonth() + 1);
      return Math.round((now - init_date) / spm * 100) / 1000;
    },

    dayRate(){
      let now = new Date();
      let startOfDay = new Date(now.getFullYear(), now.getMonth(), now.getDate());
      return Math.round((now - startOfDay) / 1000 / 86400 * 100000) / 1000;
    },
    hourRate(){
      let now = new Date();
      let startOfHour = new Date(now.getFullYear(), now.getMonth(), now.getDate(), now.getHours());
      return Math.round((now - startOfHour) / 1000 / 3600 * 10000) / 100;
    },

    minuteRate(){
      let now = new Date();
      let startOfHour = new Date(now.getFullYear(), now.getMonth(), now.getDate(), now.getHours(), now.getMinutes());
      return Math.round((now - startOfHour) / 1000 / 60 * 10000) / 100;
    },

    updateTimeBar() {
      //calculate percentage for year
      this.year_percentage = this.yearRate() + '%';
      this.$refs.yearBar.style.width = this.def_with * this.yearRate() / 100 + 'vw';

      //calculate percentage for month
      this.month_percentage = this.monthRate() + "%";
      this.$refs.monthBar.style.width = this.def_with * this.monthRate() / 100 + 'vw';


      //calculate percentage for day
      this.day_percentage = this.dayRate() + "%";
      this.$refs.dayBar.style.width = this.def_with * this.dayRate() / 100 + 'vw';

      //calculate percentage for hour
      this.hour_percentage = this.hourRate() + "%";
      this.$refs.hourBar.style.width = this.def_with * this.hourRate() / 100 + 'vw';

      //calculate percentage for minute
      this.minute_percentage = this.minuteRate() + "%";
      this.$refs.minuteBar.style.width = this.def_with * this.minuteRate() / 100 + 'vw';
    }
  },
  mounted() {
    setInterval(this.updateTimeBar, 1000);
  }
}


</script>
<style scoped>
  @import url('https://fonts.googleapis.com/css?family=Amatic+SC:400,700|Noto+Serif+TC&display=swap&subset=latin-ext');

  #time-container {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    color: var(--dark);
    overflow: hidden;
    background: var(--bright);
  }

  #time-bar-container {
    position: absolute;
    top: 12vh;
    left: 0;
    right: 0;
    height: 75vh;
    overflow: hidden;
  }

  #title {
    font-size: 10vh;
    margin-left: 10vw;
    margin-bottom: 4vh;
    font-weight: 800;
    font-family: 'Amatic SC', cursive;
  }

  #year-progress,
  #month-progress,
  #day-progress,
  #hour-progress,
  #minute-progress {
    margin-left: 10vw;
    margin-bottom: 1vh;
    font-size: 2vh;
    font-family: 'Noto Serif TC', serif;
  }

  #year-graph,
  #month-graph,
  #day-graph,
  #hour-graph,
  #minute-graph {
    height: 2vh;
    width: 80vw;
    border: 2px solid var(--broder);
    overflow: hidden;
    margin-bottom: 5vh;
    margin-left: 10vw;
  }

  #year-bar,
  #month-bar,
  #day-bar,
  #hour-bar,
  #minute-bar {
    height: 2vh;
    width: 0vw;
    background: var(--dark);
    transition: width 0.5s;
  }

  a,
  a:link,
  a:visited {
    color: var(--bright);
  }

</style>
