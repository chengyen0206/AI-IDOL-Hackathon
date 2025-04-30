<template>
  <div class="flex flex-col h-screen bg-gray-100">
    <!-- Header -->
    <header class="flex items-center justify-between px-6 py-3 shadow z-10 bg-gradient-to-r from-red-500 to-red-900">
      <div class="flex items-center space-x-2">
        <img src="@/Image/5041657E-FA68-403F-A044-69237735DE38.png" alt="Logo" class="w-8 h-8" />
        <span class="font-bold text-lg text-black">火星團花 MAX</span>
      </div>
      <div class="flex items-center space-x-4">
        <button
          class="flex items-center gap-2 px-4 py-2 rounded-full bg-gray-100 text-gray-700 hover:bg-gray-200 shadow transition-all duration-150"
          @click="clickMuted">
          <span class="material-icons text-base">{{ isMuted ? 'volume_off' : 'volume_up' }}</span>

          <span class="hidden sm:inline">靜音</span>
        </button>
        <button
          class="flex items-center gap-2 px-4 py-2 rounded-full bg-gray-100 text-gray-700 hover:bg-gray-200 shadow transition-all duration-150">
          <span class="material-icons text-base">share</span>
          <span class="hidden sm:inline">分享</span>
        </button>
        <button
          class="flex items-center gap-2 px-4 py-2 rounded-full bg-gray-100 text-gray-700 hover:bg-gray-200 shadow transition-all duration-150">
          <span class="material-icons text-base">person_add</span>
          <span class="hidden sm:inline">邀請</span>
        </button>
        <button
          class="flex items-center gap-2 px-4 py-2 rounded-full border-2 border-red-600 bg-white text-red-600 font-bold hover:bg-red-600 hover:text-white shadow transition-all duration-150">
          <span class="material-icons text-base">logout</span>
          <span class="hidden sm:inline">離開</span>
        </button>
      </div>
    </header>

    <!-- Main content -->
    <main class="flex flex-1 overflow-hidden">
      <!-- 左側 直播畫面區 -->
      <section class="flex-1 flex flex-col px-6 py-8">
        <!-- 上半部：直播畫面 -->
        <div class="flex-1 flex items-center justify-center pb-2">
          <div class="relative w-auto h-full max-h-[480px] rounded-2xl flex items-center justify-center bg-black">
            <video ref="videoRef" :muted="isMuted" autoplay loop playsinline
              class="h-full w-auto object-contain rounded-2xl" :controls="false" tabindex="-1"></video>
          </div>

        </div>
        <!-- 下半部：六張正方形照片，固定較大尺寸 -->
        <div class="flex justify-center items-center gap-10 h-32">
          <img src="@/Image/S__107880475.jpg" alt="偶像1" class="w-24 h-24 object-cover rounded-lg shadow" />
          <img src="@/Image/S__107880476.jpg" alt="偶像2" class="w-24 h-24 object-cover rounded-lg shadow" />
          <img src="@/Image/S__107880477.jpg" alt="偶像3" class="w-24 h-24 object-cover rounded-lg shadow" />
          <img src="@/Image/S__107880479.jpg" alt="偶像4" class="w-24 h-24 object-cover rounded-lg shadow" />
          <img src="@/Image/S__107880480.jpg" alt="偶像5" class="w-24 h-24 object-cover rounded-lg shadow" />
          <!--<img src="@/Image/S__107880474.jpg" alt="偶像6" class="w-24.5 h-24.5 object-cover rounded-lg shadow" />-->
        </div>
        <!-- 偶像給粉絲的話 -->
        <div class="mt-2 text-center text-sm text-gray-700 font-medium">
          謝謝大家的支持，和你們一起直播真的很幸福！🐈‍⬛🖤
        </div>
      </section>
      <!-- 右側 聊天室 -->
      <aside class="w-96 min-w-[320px] bg-white/90 border-l flex flex-col">
        <!-- 上：實況聊天室標題區，下：排行榜（有分隔線），三等分金銀銅牌縮小 -->
        <div class="bg-white/80">
          <!-- 標題區 -->
          <div class="p-3 pb-2">
            <div class="font-semibold text-base text-center text-black">實況聊天室</div>
          </div>
          <!-- 分隔線 -->
          <div class="border-t border-gray-200"></div>
          <!-- 排行榜區 -->
          <div class="p-3">
            <div class="flex flex-row justify-between text-center">
              <!-- 第一名（左） -->
              <div class="flex-1 flex flex-col items-center">
                <span class="text-xl">🥇</span>
                <span class="mt-1 text-sm font-bold text-yellow-700">火星小花</span>
              </div>
              <!-- 第二名（中） -->
              <div class="flex-1 flex flex-col items-center">
                <span class="text-xl">🥈</span>
                <span class="mt-1 text-sm font-semibold text-gray-600">Max粉一號</span>
              </div>
              <!-- 第三名（右） -->
              <div class="flex-1 flex flex-col items-center">
                <span class="text-xl">🥉</span>
                <span class="mt-1 text-sm font-semibold text-gray-600">超粉小馬</span>

              </div>
            </div>
          </div>
        </div>
        <!-- 分隔線 -->
        <div class="border-t border-gray-200"></div>
        <div class="flex-1 p-4 overflow-y-auto">
          <!-- 訊息串 ... -->
          <div v-for="(msg, index) in messages" :key="index" class="text-left text-black text-sm">
            <span class="font-bold">{{ msg.sender }}：</span>{{ msg.text }}
          </div>
        </div>
        <!-- 表情符號互動按鈕 row -->
        <div class="border-t border-gray-200 p-2">
          <div class="flex flex-wrap gap-2 justify-start">
            <button v-for="(emoji, index) in emojis" :key="index" @click="sendEmoji(emoji)"
              class="w-8 h-8 flex items-center justify-center rounded-lg hover:bg-gray-100 transition-colors duration-200"
              :title="emoji.name">
              <span class="text-xl">{{ emoji.icon }}</span>
            </button>
          </div>
        </div>
        <!-- 訊息輸入區 -->
        <div class="border-t p-3 flex items-center space-x-2">
          <input v-model="messageText"
            class="flex-1 border rounded px-3 py-2 bg-gray-300 text-white placeholder-gray-400" placeholder="發送訊息..." />
          <button @click="sendMessage"
            class="bg-red-600 text-white px-4 py-2 rounded hover:bg-red-600 transition-colors duration-200">
            發送
          </button>
        </div>
      </aside>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import Hls from "hls.js";
import axios from "axios";

const videoRef = ref(null);
const hlsUrl = "http://35.91.162.109:8088/hls/stream.m3u8";

let hls = null;
const loading = ref(true);

const messageText = ref('');
const messages = ref([]);
const playIdleMessage = async () => {
  try {
    const res = await axios.get('http://localhost:8080/idle-message');
    const { reply } = res.data;

    // 插播訊息也加進聊天室
    messages.value.push({ sender: "Max", text: reply });

  } catch (err) {
    console.error("播放待機訊息失敗", err);
  }
}
const username = ref('粉絲');

const isMuted = ref(true);

const clickMuted = () => {
  isMuted.value = !isMuted.value;
  console.log("clicked", isMuted)
}

const fanMessages = [
  "MAX 真的好帥喔❤️",
  "有人一起搶演唱會票嗎😭",
  "笑死這段超好笑哈哈哈",
  "天啊 MAX 笑起來太犯規了啦😳",
  "有生之年看到直播了嗚嗚嗚嗚🥹",
  "MAX 你的新髮型超帥欸🔥",
  "今天直播穿搭也太讚了吧！！",
  "感覺MAX今天心情超好欸～好可愛",
  "Max 笑起來像太陽一樣溫暖 ☀️",
];

const emojis = [
  { icon: '👍', name: '讚' },
  { icon: '❤️', name: '愛心' },
  { icon: '😊', name: '微笑' },
  { icon: '😂', name: '笑哭' },
  { icon: '😮', name: '驚訝' },
  { icon: '👋', name: '揮手' },
  { icon: '🎉', name: '慶祝' },
  { icon: '🌟', name: '星星' },
  { icon: '💪', name: '加油' },
  { icon: '🙏', name: '感謝' },
  { icon: '🔥', name: '火熱' },
  { icon: '👏', name: '鼓掌' },
  { icon: '🥳', name: '派對' },
  { icon: '🥰', name: '開心' },
  { icon: '🤩', name: '崇拜' },
  { icon: '😢', name: '感動' },
  { icon: '😡', name: '生氣' },
  { icon: '🤗', name: '擁抱' },
];

// 🪄 送emoji就是直接發出去當訊息
const sendEmoji = (emoji) => {
  messages.value.push({
    sender: username.value,
    text: emoji.icon
  });
};

const lastUserMessageTime = ref(Date.now());

// 🪄 送出文字訊息
const sendMessage = async () => {
  if (messageText.value.trim()) {
    const userMessage = messageText.value.trim();
    // 把自己的留言加到聊天室
    messages.value.push({ sender: username.value, text: userMessage });

    lastUserMessageTime.value = Date.now();

    try {
      const res = await axios.post('http://localhost:8080/chat-tts', {
        user_prompt: userMessage
      });

      const { reply, audio_file } = res.data;

      // 把 Max 的回覆也加進聊天室
      messages.value.push({ sender: "Max", text: reply, audio: audio_file });

      // 播放語音
      if (audio_file) {
        const audio = new Audio(audio_file);
        await audio.play();
      }
    } catch (err) {
      console.error("送出失敗", err);
      alert("發送失敗！");
    }

    messageText.value = ''; // 清空輸入框
  }
};



// 讓直播自動播放
onMounted(() => {
  if (Hls.isSupported()) {
    hls = new Hls();
    hls.loadSource(hlsUrl);
    hls.attachMedia(videoRef.value);
    videoRef.value.play();
    hls.on(Hls.Events.MANIFEST_PARSED, function () {
      loading.value = false;
    });
  } else if (videoRef.value.canPlayType("application/vnd.apple.mpegurl")) {
    videoRef.value.src = hlsUrl;
    videoRef.value.play();
    loading.value = false;
  }

  setInterval(() => {
    const randomMsg = fanMessages[Math.floor(Math.random() * fanMessages.length)];
    messages.value.push({ sender: "粉絲", text: randomMsg });

    // 自動捲到最底（可選）
    const chatContainer = document.querySelector('.flex-1.p-4.overflow-y-auto');
    if (chatContainer) {
      chatContainer.scrollTop = chatContainer.scrollHeight;
    }
  }, 5000); // 每 5 秒自動塞一則粉絲留言

  // 每 30 秒插播，但如果使用者剛發過訊息，就跳過
  setInterval(async () => {
    const now = Date.now();
    if (lastUserMessageTime && now - lastUserMessageTime < 60000) {
      console.log("使用者剛說過話，暫不插播");
      return;
    }
    await playIdleMessage();
  }, 60000);

});

onBeforeUnmount(() => {
  if (hls) {
    hls.destroy();
    hls = null;
  }
});
</script>


<style scoped>
@keyframes fade-in-down {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(30px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in-down {
  animation: fade-in-down 0.7s cubic-bezier(0.4, 0, 0.2, 1);
}

.animate-fade-in-up {
  animation: fade-in-up 0.7s cubic-bezier(0.4, 0, 0.2, 1) 0.2s both;
}

button:focus {
  outline: 2px solid #4f46e5;
  outline-offset: 2px;
}
</style>
