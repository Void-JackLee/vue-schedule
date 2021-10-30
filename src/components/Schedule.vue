<template>
  <div class="schedule">
    <div class="time-ground">
      <ul>
        <li v-for="(time , index) in pageTimeGround" v-bind:key="index"
            :style="`--unitPerHeight: ${unit.height}px`">
          <span :style="`left: ${unit.width / 2}%`">{{time}}</span>
          <p :style="timeListSty"></p>
        </li>
      </ul>
    </div>
    <div class="task-ground">
      <ul>
        <li class="task-list task-list-blank"
            :style="`width: ${unit.width}%;`"/>
        <li v-for="(week, index) in weekGround" class="task-list" v-bind:key="index"
            :style="'width: ' + unit.width + '%'">
          <p
              :style="`--unitPerHeight: ${unit.height}px`"
          >{{week}}</p>
          <ul :style="taskListSty">
            <li class="task-list-item" v-for="(detail,idx) in taskDetail[index]" :style="detail.styleObj" @click="showDetail(detail, week)" v-bind:key="idx">
              <p>{{detail.dateStart}} - {{detail.dateEnd}}</p>
              <h3>{{detail.title}}</h3>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <div :style="`height: ${parseInt(taskListSty.height.substr(0,taskListSty.height.length - 2)) + unit.height}px`"></div>

    <modal :show="showModal" :show-modal-detail="showModalDetail"> </modal>
  </div>
</template>

<style lang="scss" scoped>

  p {
    margin: 0;
  }

  ol, ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

	.schedule {
		width: 100%;
		max-width: 1400px;
		margin: 0 auto;
		position: relative;
	}
	.time-ground {
		display: block;
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;

    ul {
      li {
        margin-top: calc(var(--unitPerHeight) / 2);
        font-size: 1rem;
        height: calc(var(--unitPerHeight) / 2);

        span {
          position: absolute;
          transform: translate(-50%,-50%);
        }

        p {
          position:absolute;
          left: 0;

          height: 1px;
          background-color: #EAEAEA;
        }
      }

    }
	}


	.task-ground {
		width: 100%;
	}


	.task-list{
		float: left;
		/*width: 20%;*/
		box-sizing: border-box;
		border: 1px solid #EAEAEA;

    p {
      text-align: center;
      font-size: 1rem;
      height: calc(var(--unitPerHeight) / 2);
      line-height: calc(var(--unitPerHeight) / 2);
    }
	}

  .task-list-blank {
    border: 1px solid rgba(0,0,0,0);
  }


	.task-list-item{
		position: absolute;
		background-color: #577F92;
		/*width: 20%;*/
		height: 20px;
		cursor: pointer;

    p {
      text-align: left;
      padding: 0;
      margin: 1rem 0 0 1rem;
      font-size: 0.8rem;
      color: #EDF2F6;
    }

    h3 {
      color: #E0E7E9;
      margin: 1rem 0 0 1rem;
    }
	}
</style>

<style>

</style>

<script>
const data = {
  pageTimeGround: [],
  showModal: false,
  showModalDetail: {
    dateStart: '09:30',
    dateEnd: '10:30',
    title: 'Metting',
    week: 'Monday',
    styleObj: {
      backgroundColor: "#903749"
    },
    detail: 'Metting (German: Mettingen) is a commune in the Moselle department in Grand Est in north-eastern France.'
  },
  taskListSty: {
    height: '900px'
  },
  timeListSty: {
    width: '100%',
    marginLeft: '20%'
  },

  unit: {
    height: 100, // px
    width: 20 // %
  }

}


import Modal from './Modal.vue';
export default {
	name: 'Schedule',
	props: {
		timeGround: {
			type: Array,
			default: () => []
		},
		weekGround: {
			type: Array,
			default: () => [
				'Monday',
				'Tuesday',
				'Wednesday',
				'Thursday',
				'Friday'
			]
		},
		taskDetail: {
			type: Array,
			default: () => []
		},
		color: {
			type: Array,
			default(){
				return [
					"#2B2E4A",
					"#521262",
					"#903749",
					"#53354A",
					"#40514E",
					"#537780",
				]
			}
		}
	},
	components: {
		Modal: Modal
	},
	// watch: {
	// 	timeGround: function(value){
	// 		this.timeGround = value
	// 	}
	// },
	watch: {
		timeGround(value) {

				// console.log('value=', value);
				this.pageTimeGround = this.initTimeGroud(value);
				// return value;
		}
	},
	data() {
		return data
	},
	created() {
    this.unit.width = (100 / (this.weekGround.length + 1));
    this.timeListSty.width = this.unit.width * this.weekGround.length + '%'
    this.timeListSty.marginLeft = this.unit.width + "%"
		// console.log(this.ta)
		this.pageTimeGround = this.initTimeGroud(this.timeGround);

		let maxTime = this.pageTimeGround[this.pageTimeGround.length - 1];
		let minTime = this.pageTimeGround[0];
		let maxMin = maxTime.split(':')[0] * 60 + maxTime.split(':')[1] * 1;
		let minMin = minTime.split(':')[0] * 60 + minTime.split(':')[1] * 1;
		// console.log(maxMin);
		// console.log(minMin);
		// console.log(maxTime);
		for (let i = 0; i < this.taskDetail.length; i++) {
      for (let j = 0; j < this.taskDetail[i].length; j++) {
        // console.log(this.taskDetail[i][j]);
        let startMin = this.taskDetail[i][j].dateStart.split(':')[0] * 60 + this.taskDetail[i][j].dateStart.split(':')[1] * 1;
        let endMin = this.taskDetail[i][j].dateEnd.split(':')[0] * 60 + this.taskDetail[i][j].dateEnd.split(':')[1] * 1;
        if(startMin < minMin || endMin > maxMin) {
          this.taskDetail[i].splice(j, 1);
          j--;
          continue
        }
        // console.log(endMin);
        let difMin = endMin - startMin;
        console.log(difMin)
        // console.log(startMin);
        // console.log(endMin);
        this.taskDetail[i][j].styleObj = {
          height: difMin * this.unit.height / 60 + 'px',
          width: this.unit.width + '%',
          top: ((startMin - (this.pageTimeGround[0].split(":")[0] * 60 + this.pageTimeGround[0].split(":")[1] * 1)) * this.unit.height / 60) + this.unit.height / 2 + 'px',
          backgroundColor: this.color[~~(Math.random() * this.color.length)]
        }
        // console.log(this.color[~~(Math.random() * 4)]);
        // console.log(this.taskDetail);
        // console.log(this.timeGround);
      }
		}
		console.log(this.taskDetail);
	},
	mounted() {
		this.taskListSty.height = (this.pageTimeGround.length - 1) * this.unit.height + 'px';
		// this.timeListSty.width = this.weekGround.length * 14 + '%';

		// console.log(this.taskDetail);
		// console.log(this.weekGround);
	},
	methods: {
		showDetail(obj, week){
			obj.week = week;
			this.showModalDetail = obj;
			this.showModal = !this.showModal;
		},
		initTimeGroud(value){
			if(value && value.length == 2){
					let startTime = value[0].split(":")[0] * 1;
					let endTime = value[1].split(":")[0] * 1;
					value = [];
					for(let i = startTime; i <= endTime; i++){
						// console.log(1);
						// value.push()
						let hour = i < 10 ? "0" + i : "" + i;
						value.push(hour + ":00");
					}
				}else{
					value = [
						"09:00",
						"10:00",
						"11:00",
						"12:00",
						"13:00",
						"14:00",
						"15:00",
						"16:00",
						"17:00",
						"18:00",
					]
				}
				return value;
		}
	}
}
</script>
