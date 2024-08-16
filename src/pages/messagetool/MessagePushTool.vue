<template>
  <el-form
    :model="form"
    :label-width="labelWidth"
    class="form"
    :rules="rules"
    ref="form"
  >
    <el-form-item label="msgId" prop="msgId">
      <el-input
        type="number"
        v-model="form.msgId"
        placeholder="msgId"
      ></el-input>
    </el-form-item>

    <el-form-item label="标题">
      <el-input v-model="form.title" placeholder="标题"></el-input>
    </el-form-item>

    <el-form-item label="内容" prop="content">
      <el-input v-model="form.content" placeholder="内容"></el-input>
    </el-form-item>

    <el-form-item label="接收者" prop="recId" required>
      <el-input v-model="form.recId" placeholder="内容"></el-input>
    </el-form-item>

    <el-form-item label="手机号" prop="mobile">
      <el-input v-model="form.mobile" placeholder="手机号"></el-input>
    </el-form-item>

    <el-form-item label="email" prop="email">
      <el-input v-model="form.email" placeholder="email"></el-input>
    </el-form-item>

    <el-form-item label="附加数据">
      <complex-input v-bind:kv="extData" />
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="sendMsg()">确 定</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import ComplexInput from "../modules/ComplexInput";
export default {
  name: "MessagePushTool",
  components: { ComplexInput },
  data() {
    return {
      extData: [],
      labelWidth: "120px",
      recIdInputVisible: false,
      mobileInputVisible: false,
      emailInputVisible: false,
      recIdInputValue: "",
      mobileInputValue: "",
      emailInputValue: "",
      form: {
        ext: {},
        jgExt: {},
        recIds: [],
        mobiles: [],
        emails: [],
      },
      rules: {
        msgId: [{ required: true, message: "请输入 msgId", trigger: "blur" }],
        content: [
          { required: true, message: "请输入推送内容", trigger: "blur" },
        ],
      },
    };
  },
  methods: {
    handleRecIdClose(tag) {
      this.form.recIds.splice(this.form.recIds.indexOf(tag), 1);
    },

    handleMobileClose(tag) {
      this.form.mobiles.splice(this.form.mobiles.indexOf(tag), 1);
    },

    handleEmailClose(tag) {
      this.form.emails.splice(this.form.emails.indexOf(tag), 1);
    },

    showRecIdInput() {
      this.recIdInputVisible = true;
      this.$nextTick((_) => {
        this.$refs.saveRecIdTagInput.$refs.input.focus();
      });
    },

    showMobileInput() {
      this.mobileInputVisible = true;
      this.$nextTick((_) => {
        this.$refs.saveMobileTagInput.$refs.input.focus();
      });
    },

    showEmailInput() {
      this.emailInputVisible = true;
      this.$nextTick((_) => {
        this.$refs.saveEmailTagInput.$refs.input.focus();
      });
    },

    recIdHandleInputConfirm() {
      let recIdInputValue = this.recIdInputValue;

      if (this.form.recIds.indexOf(recIdInputValue) > -1) {
        this.msgShow.errMsgShow(this, "标签不允许重复");
        return;
      }

      if (recIdInputValue) {
        this.form.recIds.push(recIdInputValue);
      }

      this.recIdInputVisible = false;
      this.recIdInputValue = "";
    },

    mobileHandleInputConfirm() {
      let mobileInputValue = this.mobileInputValue;

      if (this.form.mobiles.indexOf(mobileInputValue) > -1) {
        this.msgShow.errMsgShow(this, "手机号不允许重复");
        return;
      }

      if (mobileInputValue) {
        this.form.mobiles.push(mobileInputValue);
      }

      this.mobileInputVisible = false;
      this.mobileInputValue = "";
    },

    emailHandleInputConfirm() {
      let emailInputValue = this.emailInputValue;

      if (this.form.emails.indexOf(emailInputValue) > -1) {
        this.msgShow.errMsgShow(this, "Email 不允许重复");
        return;
      }

      if (emailInputValue) {
        this.form.emails.push(emailInputValue);
      }

      this.emailInputVisible = false;
      this.emailInputValue = "";
    },

    convertExtData() {
      this.form.ext = this.extData
        .filter((item) => item["key"] !== "" && item["value"] !== "")
        .reduce(
          (obj, item) => Object.assign(obj, { [item.key]: item.value }),
          {}
        );
    },

    sendMsg() {
      this.convertExtData();

      this.apiRequest
        .post(this.url.data.tool.send, this.form, {
          headers: {
            "Content-Type": "application/json;charset=UTF-8",
          },
        })
        .then(() => {
          this.msgShow.sucMsgShow(this, "消息发送成功");
        });
    },
  },
};
</script>

<style scoped>
.form {
  width: 500px;
  margin-top: 20px;
}

.el-tag + .el-tag {
  margin-left: 10px;
}

.button-new-tag {
  margin-left: 10px;
  height: 32px;
  line-height: 30px;
  padding-top: 0;
  padding-bottom: 0;
}

.input-new-tag {
  width: 300px;
  margin-left: 10px;
  vertical-align: bottom;
}
</style>