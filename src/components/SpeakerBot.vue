<template>
  <div id="widgetBodyId" class="widgetBody collapsed">
    <div class="widgetHeader">
      <div class="widgetControls">
        <span @click="collapse" class="collapse"></span>
      </div>
      <div class="botViewBlock">
        <div class="botView"></div>
        <div class="botName">{{ botName }}</div>
      </div>
    </div>
    <div id="workSpaceId" class="widgetWorkSpace">
      <div v-if="startDialogue === null" class="msgBlockBot">
        <span class="botMessageFramePreload">
          <img class="msgPreloader" src="../assets/msgPreloaader.svg" />
        </span>
      </div>
      <div class="msgBlockBot">
        <span v-if="startDialogue" class="botMessageFrame">
          Привет! Что я могу для Вас сделать?
        </span>
      </div>
      <div class="msgBlockBot">
        <span
          @click="orderPizzaAction"
          v-if="startDialogue"
          class="botMessageFrame botAction"
        >
          Заказать пиццу
        </span>
      </div>
      <div class="msgBlockBot">
        <span
          @click="alarmClockAction"
          v-if="startDialogue"
          class="botMessageFrame botAction"
        >
          Установить будильник
        </span>
      </div>
      <div class="msgBlockBot">
        <span
          @click="weatherAction"
          v-if="startDialogue"
          class="botMessageFrame botAction"
        >
          Вывести погоду
        </span>
      </div>
      <div v-for="(el, i) in messages" :key="i" :class="el.parentBlockClass">
        <span :class="el.msgClass">
          {{ el.msg }}
        </span>
      </div>
      <div class="msgBlockUser">
        <span v-if="userType === true" class="botMessageFramePreload">
          <img class="msgPreloader" src="../assets/msgPreloaader.svg" />
        </span>
      </div>
      <div v-if="botTyping == true" class="msgBlockBot">
        <span class="botMessageFramePreload">
          <img class="msgPreloader" src="../assets/msgPreloaader.svg" />
        </span>
      </div>
    </div>
    <div id="footerId" class="widgetFooter">
      <textarea
        v-model="currentUserMsg"
        class="widgetInput"
        @keydown.ctrl.enter="sendMsg"
        @input="userTyping"
        type="text"
        id="msgInput"
      ></textarea>
      <button type="button" @click="sendMsg" class="sendButton">
        Отправить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SpeakerBot",
  data() {
    return {
      botName: "Ассистент",
      currentUserMsg: "",
      messages: [],
      startDialogue: null,
      userType: false,
      botTyping: false,
    };
  },
  methods: {
    collapse(e) {
      document.getElementById("widgetBodyId").classList.toggle("collapsed");
      e.target.classList.toggle("spinCaret");
      setTimeout(() => {
        this.startDialogue = 1;
      }, 1800);
    },
    userTyping() {
      const textArea = document.getElementById("msgInput");
      if (textArea.value) {
        this.userType = true;
      } else {
        this.userType = false;
      }
      const workSpace = document.getElementById("workSpaceId");
      workSpace.scrollTop = workSpace.scrollHeight;
    },
    sendMsg() {
      if (this.currentUserMsg) {
        this.messages.push({
          msg: this.currentUserMsg,
          side: "user",
          parentBlockClass: "msgBlockUser",
          msgClass: "userMessageFrame",
        });
      }
      if (this.currentUserMsg == "Выведи погоду") {
        this.botTyping = true;
        setTimeout(() => {
          this.messages.push(
            {
              msg: "Уже вывожу!",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            },
            {
              msg: "Что ещё я могу для вас сделать?",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            }
          );
          this.botTyping = false;
        }, 1500);
        setTimeout(() => {
          const workSpace = document.getElementById("workSpaceId");
          workSpace.scrollTop = workSpace.scrollHeight;
        }, 1501);
      }
      if (this.currentUserMsg == "Закажи пиццу") {
        this.botTyping = true;
        setTimeout(() => {
          this.messages.push(
            {
              msg: "Уже заказываю!",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            },
            {
              msg: "Возможно, я чем-нибудь ещё могу помочь?",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            }
          );
          this.botTyping = false;
        }, 1500);
        setTimeout(() => {
          const workSpace = document.getElementById("workSpaceId");
          workSpace.scrollTop = workSpace.scrollHeight;
        }, 1501);
      }
      if (this.currentUserMsg == "Установи будильник") {
        this.botTyping = true;
        setTimeout(() => {
          this.messages.push(
            {
              msg: "Уже установил!",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            },
            {
              msg: "Как я мог бы помочь вам ещё?",
              side: "bot",
              parentBlockClass: "msgBlockBot",
              msgClass: "botMessageFrame",
            }
          );
          this.botTyping = false;
        }, 1500);
        setTimeout(() => {
          const workSpace = document.getElementById("workSpaceId");
          workSpace.scrollTop = workSpace.scrollHeight;
        }, 1501);
      }
      this.currentUserMsg = "";
      this.userType = false;
      setTimeout(() => {
        const workSpace = document.getElementById("workSpaceId");
        workSpace.scrollTop = workSpace.scrollHeight;
      }, 50);
    },
    weatherAction() {
      this.currentUserMsg = "Выведи погоду";
      this.sendMsg();
    },
    alarmClockAction() {
      this.currentUserMsg = "Установи будильник";
      this.sendMsg();
    },
    orderPizzaAction() {
      this.currentUserMsg = "Закажи пиццу";
      this.sendMsg();
    },
  },
};
</script>

<style scoped>
.widgetBody {
  border: 1px solid lightgray;
  width: 325px;
  height: 360px;
  border-radius: 6px;
  position: fixed;
  right: 15px;
  bottom: 25px;
  -webkit-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  -webkit-transition: 0.3s ease;
  -o-transition: 0.3s ease;
  transition: 0.3s ease;
}

.widgetHeader {
  width: 100%;
  border-bottom: 1px solid lightgray;
}

.botViewBlock {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}

.botView {
  height: 50px;
  width: 50px;
  margin-left: 10px;
  background-image: url("../assets/botAvatar.png");
  background-repeat: no-repeat;
  background-size: cover;
}

.botName {
  margin-left: 10px;
  font-size: 20px;
  font-weight: 400;
}

.widgetControls {
  position: absolute;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: space-evenly;
  -ms-flex-pack: space-evenly;
  justify-content: space-evenly;
  border-bottom: 1px solid lightgray;
  border-left: 1px solid lightgray;
  right: 0px;
  top: 0px;
  padding: 3px;
  width: 30px;
  height: 20px;
  border-radius: 0 6px 0 6px;
}

.collapse {
  display: block;
  cursor: pointer;
  width: 25px;
  height: 25px;
  background-image: url("../assets/caret.svg");
  background-repeat: no-repeat;
  background-size: cover;
}

.collapsed {
  height: 50px !important;
  -webkit-transition: 0.3s ease;
  -o-transition: 0.3s ease;
  transition: 0.3s ease;
  overflow: hidden;
}

.spinCaret {
  -webkit-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  transform: rotate(180deg);
}

.widgetWorkSpace {
  overflow: auto;
  height: 60%;
  padding: 4px;
  -webkit-transition: 0.3s ease;
  -o-transition: 0.3s ease;
  transition: 0.3s ease;
  border-bottom: 1px solid lightgray;
}

.msgBlockBot {
  width: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: start;
  -ms-flex-pack: start;
  justify-content: flex-start;
}

.msgBlockUser {
  position: sticky;
  width: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: end;
  -ms-flex-pack: end;
  justify-content: flex-end;
}

.botMessageFrame {
  word-wrap: break-word;
  color: #fff;
  margin: 4px;
  display: block;
  padding: 8px;
  border-radius: 6px 6px 6px 0;
  width: -webkit-fit-content;
  width: -moz-fit-content;
  width: fit-content;
  max-width: 90%;
  height: auto;
  border: 1px solid lightgray;
  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    from(rgba(33, 109, 255, 1)),
    color-stop(25%, rgba(72, 91, 255, 1)),
    color-stop(50%, rgba(115, 71, 255, 1)),
    color-stop(75%, rgba(158, 51, 255, 1)),
    to(rgba(195, 36, 255, 1))
  );
  background: -o-linear-gradient(
    top,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  background: linear-gradient(
    180deg,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  -webkit-transition: 0.1s;
  -o-transition: 0.1s;
  transition: 0.1s;
}

.botAction {
  cursor: pointer;
}

.botAction:hover {
  -webkit-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  -webkit-transform: scale(1.011111);
  -ms-transform: scale(1.011111);
  transform: scale(1.011111);
  -webkit-transition: 0.1s;
  -o-transition: 0.1s;
  transition: 0.1s;
}

.botMessageFramePreload {
  word-break: break-all;
  word-wrap: break-word;
  color: #fff;
  margin: 4px;
  display: block;
  padding: 0 6px 0 6px;
  border-radius: 6px 6px 6px 0;
  max-width: 15%;
  height: auto;
  border: 1px solid lightgray;
  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    from(rgba(33, 109, 255, 1)),
    color-stop(25%, rgba(72, 91, 255, 1)),
    color-stop(50%, rgba(115, 71, 255, 1)),
    color-stop(75%, rgba(158, 51, 255, 1)),
    to(rgba(195, 36, 255, 1))
  );
  background: -o-linear-gradient(
    top,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  background: linear-gradient(
    180deg,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
}

.userMessageFrame {
  word-break: break-all;
  word-wrap: break-word;
  color: #fff;
  margin: 4px;
  display: block;
  padding: 8px;
  border-radius: 6px 6px 0 6px;
  width: -webkit-fit-content;
  width: -moz-fit-content;
  width: fit-content;
  max-width: 90%;
  height: auto;
  border: 1px solid lightgray;
  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    from(rgba(33, 109, 255, 1)),
    color-stop(25%, rgba(72, 91, 255, 1)),
    color-stop(50%, rgba(115, 71, 255, 1)),
    color-stop(75%, rgba(158, 51, 255, 1)),
    to(rgba(195, 36, 255, 1))
  );
  background: -o-linear-gradient(
    top,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  background: linear-gradient(
    180deg,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  -webkit-transition: 0.1s;
  -o-transition: 0.1s;
  transition: 0.1s;
}

.msgPreloader {
  width: 35px;
}

.widgetFooter {
  width: 100%;
  height: auto;
  -webkit-transition: 0.3s ease;
  -o-transition: 0.3s ease;
  transition: 0.3s ease;
}

.widgetInput {
  margin-top: 9px;
  border: 1px solid lightgray;
  border-radius: 6px;
  height: 40px;
  width: 90%;
  resize: none;
  padding: 5px;
  outline: none;
}

.sendButton {
  border-radius: 6px;
  cursor: pointer;
  margin-top: 2px;
  height: 25px;
  width: 100%;
  color: #fff;
  background: -webkit-gradient(
    linear,
    left top,
    left bottom,
    from(rgba(33, 109, 255, 1)),
    color-stop(25%, rgba(72, 91, 255, 1)),
    color-stop(50%, rgba(115, 71, 255, 1)),
    color-stop(75%, rgba(158, 51, 255, 1)),
    to(rgba(195, 36, 255, 1))
  );
  background: -o-linear-gradient(
    top,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
  background: linear-gradient(
    180deg,
    rgba(33, 109, 255, 1) 0%,
    rgba(72, 91, 255, 1) 25%,
    rgba(115, 71, 255, 1) 50%,
    rgba(158, 51, 255, 1) 75%,
    rgba(195, 36, 255, 1) 100%
  );
}

.sendButton:hover {
  -webkit-box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
  box-shadow: 0px 5px 10px 2px rgba(34, 60, 80, 0.2);
}
</style>
