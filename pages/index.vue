<template>
  <div class="min-h-screen bg-slate-800">
    <div class="w-screen h-14 bg-slate-900 pt-3 pl-3 flex justify-between">
      <div>
        <h1 class="text-white text-xl">uMarkdown</h1>
      </div>
      <div>
        <span
          class="material-icons text-white text-2xl mr-6"
          @click="delete_storage"
        >
          delete
        </span>
        <span
          class="material-icons text-white text-2xl mr-6"
          @click="download_md"
        >
          file_download
        </span>
        <span class="material-icons text-white text-2xl mr-6" @click="download_html"> html </span>
      </div>
    </div>
    <div class="flex p-4 min-h-full">
      <div class="w-1/2 h-full p-2">
        <textarea
          class="w-full h-96 bg-slate-700 text-white p-3 rounded-md"
          name=""
          id=""
          v-model="md"
        ></textarea>
      </div>
      <div class="w-1/2 p-2">
        <div class="w-full min-h-96 bg-slate-700 text-white p-3 rounded-md">
          <div class="ou" v-html="html"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      md: "",
      html: "",
    };
  },
  mounted: function () {
    this.md = localStorage.getItem("md");
    if (this.md == null) {
      this.md = "";
    }
  },
  methods: {
    delete_storage: function () {
      if (window.confirm("内容は復元できません。削除してもよろしいですか？")) {
        localStorage.removeItem("md");
        location.reload();
      }
    },
    download_md: function () {
      var blob = new Blob([this.md], { type: "text/plain" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      document.body.appendChild(a);
      a.download = "text.md";
      a.href = url;
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    },
    download_html: function () {
      var blob = new Blob([this.html], { type: "text/html" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      document.body.appendChild(a);
      a.download = "text.html";
      a.href = url;
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    },
  },
  watch: {
    md: function (new_val, old_val) {
      this.html = marked.parse(new_val);
      localStorage.setItem("md", new_val);
    },
  },
};
</script>

<style lang="scss">
textarea:focus {
  outline: 0;
}
.ou {
  h1 {
    font-size: 2rem;
  }
  h2 {
    font-size: 1.8rem;
  }
  h3 {
    font-size: 1.6rem;
  }
  h4 {
    font-size: 1.4rem;
  }
  h5 {
    font-size: 1.3rem;
  }
  h6 {
    font-size: 1.2rem;
  }
  ul {
    list-style: disc;
    padding-left: 20px;
  }
  ol {
    list-style-type: decimal;
  }
  ol {
    padding-left: 20px;
  }
  blockquote {
    padding: 4px;
    padding-left: 10px;
    background-color: #475569;
  }
  code {
    padding: 4px;
    background-color: #475569;
  }
  pre {
    background-color: #475569;
    width: 100%;
    padding: 8px;
  }
  a {
    color: #38bdf8;
  }
}
</style>