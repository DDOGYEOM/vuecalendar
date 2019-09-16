<template>
  <div id="app">
    <div class="logo">
      <h2>캘린더 앱</h2>
    </div>
    <div class="button-row">
      <img src="/img/prev.png" class="prev btn" @click="prev">
      <div class="month-label">{{current.getMonth() + 1}} 월</div>
      <img src="/img/prev.png" class="next btn" @click="next">
    </div>
<div class="container">
    <div class="weeklist">
      <div class="weekday"><a>일</a></div>
      <div class="weekday"><a>월</a></div>
      <div class="weekday"><a>화</a></div>
      <div class="weekday"><a>수</a></div>
      <div class="weekday"><a>목</a></div>
      <div class="weekday"><a>금</a></div>
      <div class="weekday"><a>토</a></div>
    </div>
    <div class="calendar">
      <div v-for="week in dayList" class="week">
        <day
          v-for="day in week"
          :day="day"
          :key="day.idx"
          @edit="openEditPopup"
          @open="openPopupWindow(day.idx)"
        >{{day}}</day>
      </div>
    </div>
</div>
    <div id="popup" v-if="popupOpen">
      <div class="inner">
        <div class="form-group">
          <input v-model="todoTitle" type="text" placeholder="할 일을 입력하세요" />
          <textarea v-model="todoContent" placeholder="상세한 내용을 입력하세요"></textarea>
          <button type="button" @click="saveTodo">입력</button>
          <button type="button" @click="popupOpen = false">닫기</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  created() {
    let now = new Date();
    this.drawCalendar(now);
  },
  methods: {
    drawCalendar(now) {
      this.current = now;
      let first = now.getFirstDate();
      first.addDay(-first.getDay()); //달력의 최초 시작일이 나온다.
      this.dayList = [];
      while (true) {
        let arr = [];
        for (let i = 0; i < 7; i++) {
          arr.push({ idx: first.getTime(), date: first.clone(), list: [] });
          first.addDay(1);
        }
        this.dayList.push(arr);
        if (first.getMonth() != now.getMonth()) {
          break;
        }
      }
    },
    prev() {
      let pre = this.current.clone();
      pre.setMonth(pre.getMonth() - 1);
      this.drawCalendar(pre);
    },
    next() {
      let nex = this.current.clone();
      nex.setMonth(nex.getMonth() + 1);
      this.drawCalendar(nex);
    },
    openPopupWindow(day) {
      console.log(day);
      this.currentPopupData = day;
      this.popupOpen = true;
    },
    saveTodo() {
      if (this.currentEditItem != null) {
        this.currentEditItem.name = this.todoTitle;
        this.currentEditItem.content = this.todoContent;
        this.currentEditItem = null;
      } else {
        this.dayList.forEach(week => {
          week.forEach(day => {
            if (day.idx == this.currentPopupData) {
              day.list.push({
                name: this.todoTitle,
                content: this.todoContent
              });
            }
          });
        });
      }

      this.popupOpen = false;
      this.todoTitle = "";
      this.todoContent = "";
    },
    openEditPopup(e, idx, dIdx) {
      this.dayList.forEach(week => {
        week.forEach(day => {
          if (day.idx == dIdx) {
            this.todoTitle = day.list[idx].name;
            this.todoContent = day.list[idx].content;
            this.currentEditItem = day.list[idx];
            this.popupOpen = true;
          }
        });
      });
    }
  },
  data() {
    return {
      current: null,
      dayList: [],
      popupOpen: false,
      currentPopupData: null,
      todoTitle: "",
      todoContent: "",
      currentEditItem: null
    };
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Nanum+Pen+Script&display=swap');

* {
   font-family: 'Nanum Pen Script', cursive;
   font-size: 30px;
   box-sizing: border-box;
}


#app {
  background-color: #FEE2C3;
}

.logo {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.button-row {
  width: 1440px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
}

.btn {
  width: 50px;
}

.next {
  transform : rotate(180deg); 
}

.container {
  width: 1800px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 0 auto;
}

.weeklist {
  width: 1550px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  margin-top: 20px; 
  margin-bottom: 0px;
}

.weekday {
  width: 206px;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #E68F71;
  border-bottom: 0;
  border-radius: 15px 15px 0 0;
  border-right: 0;
  margin-bottom: 2px;
  border: 2px solid #67493C;
}

.month-label {
  font-size: 25px;
}
.calendar {
  border: 2px solid #67493C;
  border-top: 0;
  width: 80%;
  margin: 0 auto;
  background-color: #FDCA95;
  border-left: 0;
}
.week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: minmax(120px, auto);
}

#popup {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

#popup > .inner {
  width: 500px;
  height: 400px;
  background-color: rgb(255, 223, 189);
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

#popup > .inner > .form-group {
  display: grid;
  grid-template-columns: 1fr;
  width: 80%;
  height: 80%;
  grid-gap: 10px;
}

.form-group > * {
  font-size: 20px;
  border: 0;
}

.form-group > input, .form-group > textarea {
  border-radius: 5px;
  padding-left: 10px;
}

.form-group > textarea {
  height: 150px;
}

.form-group > button {
  border-radius: 10px;
}

.form-group :nth-child(3) {
  background-color: rgb(181, 190, 240);
}
.form-group :nth-child(4) {
  background-color: rgb(240, 181, 184);
}

</style>

