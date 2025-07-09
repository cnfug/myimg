<template>
  <footer class="w-full max-w-[1666px] mt-36 pb-4">
    <main class="m-auto pt-4 flex flex-col-reverse md:flex-row justify-between gap-2 md:gap-16 w-full max-w-[1666px] border-t">
      <section class="[&>p]:text-xs [&>p]:w-full [&>p]:py-[6px] [&>p]:text-center md:[&>p]:text-left [&>p>a]:text-slate-400">
        <p class="text-red-600">注意:上传违反中国大陆、香港及美国法律的图片将会直接删除，并封禁设备IP。</p>
        <p class="flex gap-2 justify-center md:justify-start">
          <a href="https://pages.cloudflare.com"  target="_blank" rel="noopener noreferrer">
            Cloudflare Pages
          </a>
          <a href="https://www.cloudflare.com/zh-cn/application-services/products/cdn/"  target="_blank" rel="noopener noreferrer">
            Cloudflare CDN 
          </a>
          <a href="https://vuejs.org"  target="_blank" rel="noopener noreferrer">
            Vue.js  
          </a>
        </p>
        <p id="cf" class="text-xs text-gray-500"></p>
      </section>
    </main>
  </footer>
</template>
 
<script>
export default {
  name: 'Footer',
  mounted() {
    this.updateCloudflareInfo(); 
  },
  methods: {
    async updateCloudflareInfo() {
      try {
        const response = await fetch("https://cloudflare.com/cdn-cgi/trace"); 
        if (response.ok)  {
          const data = await response.text(); 
          const lines = data.split("\n"); 
          const info = {};
          lines.forEach((line)  => {
            const parts = line.split("="); 
            if (parts.length  === 2) {
              info[parts[0]] = parts[1];
            }
          });
          const displayText = [
            info.loc, 
            info.ip, 
            info.colo, 
            info.http, 
            info.visit_scheme, 
            info.tls, 
            info.kex  
          ].filter(Boolean).join(" | ");
          
          const cfElement = document.getElementById("cf"); 
          if (cfElement) {
            cfElement.textContent  = displayText;
          }
        }
      } catch (error) {
        console.error(" 获取Cloudflare节点信息失败:", error);
      }
    }
  }
};
</script>
