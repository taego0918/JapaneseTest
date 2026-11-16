<template>
    <div style="text-align: center;">
        <!-- 確保 data 已經載入完成才顯示 -->
        <div style="text-align: center;" v-if="data && data.length">
            <p class="topic" v-html="randomItem.topic"></p>
            <!-- 用洗牌後的 shuffledItems 印出選項 -->
            <p class="option-item"
                :class="{
                    'wrong-item': option.isWrong,
                    'correct-item': isClicked && option.B
                }" 
                @click="onClick(option)"
                v-for="(option, index) in shuffledItems"
                :key="index">
                {{ option.A }}
            </p>
            <!-- 用 v-html 讓 <br/> 換行生效 -->
            <p class="explain" v-show="isClicked" v-html="randomItem.explain"></p>
        </div>
        <p v-else>載入中...</p>

        <!-- 點擊按鈕可以再次隨機抽一筆 -->
        <button @click="getRandomItem">下一題</button>
    </div>
</template>

<script setup>
    import { ref, computed, watch } from 'vue';

    // 1. 直接靜態引入各個級別的 JS 題庫檔案（支援註解）
    import N4HiRaGaNa from '../../assets/data/N4/HiRaGaNa.js';
    import N4GrammarSelection from '../../assets/data/N4/GrammarSelection.js';

    // 2. 建立題庫對應表
    const quizDatabases = {
        N4HiRaGaNa,
        N4GrammarSelection
    };

    // 3. 取得從 app.vue 傳過來的全域狀態
    const level = useState('quizLevel');
    const testType = useState('quizType');

    // 4. 根據當前 level 初始化對應的資料
    const data = ref(quizDatabases[level.value + testType.value] || []);
    const isClicked = ref(false);
    const currentIndex = ref(0);

    const initIndex = () => {
        isClicked.value = false;
        if (data.value && data.value.length > 0) {
            console.log(data.value.length);
            currentIndex.value = Math.floor(Math.random() * data.value.length);
        }
    };
    initIndex();

    // 5. 關鍵：當上方切換 level 時，自動切換對應的 JS 題庫並重設索引
    watch(level, (newLevel) => {
        data.value = quizDatabases[newLevel + testType.value] || [];
        initIndex();
    });

    watch(testType, (newType) => {
        data.value = quizDatabases[level.value + newType] || [];
        initIndex();
    });

    const onClick = (option) => {
        if (isClicked.value) return;
        if (option.B === false) option.isWrong = true;
        isClicked.value = true;
    };

    const getRandomItem = () => {
        if (!data.value || data.value.length === 0) return;

        if (randomItem.value && randomItem.value.item) {
            randomItem.value.item.forEach(opt => {
                opt.isWrong = false;
            });
        }

        isClicked.value = false;
        
        let newIndex;
        do {
            newIndex = Math.floor(Math.random() * data.value.length);
        } while (newIndex === currentIndex.value && data.value.length > 1);
        currentIndex.value = newIndex;
    };

    const randomItem = computed(() => {
        if (!data.value || data.value.length === 0) return null;
        return data.value[currentIndex.value];
    });

    const shuffledItems = computed(() => {
        if (!randomItem.value || !randomItem.value.item) return [];
        const itemsCopy = [...randomItem.value.item];
        return itemsCopy.sort(() => Math.random() - 0.5);
    });
</script>

<style scoped>
    p {color:#ffffff;}
    h1 {
        color: #00dc82;
    }
    button {
        padding: 8px 16px;
        cursor: pointer;
        background-color: #252530;
        color: white;
        border: 1px solid #353848;
        border-radius: 4px;
    }
    .topic{
        margin: auto;
        max-width: 500px;
        width: 100%;
        text-align: left;
    }
    .option-item{
        margin: 10px auto;
        max-width: 500px;
        width: 100%;
        padding: 10px;
        border-radius:5px;
        border: 1px solid #353848;
        background-color: #252530;
        cursor: pointer;
        text-align: left;
    }
    .wrong-item{
        background-color: #391d1c;
        color: #bd5659;
        font-weight: bold;
    }
    .correct-item{
        background-color: #1a2c24;
        color:#64b391;
        font-weight: bold;
    }
    .explain{
        margin: auto;
        max-width: 500px;
        width: 100%;
        padding: 10px;
        border-radius:5px;
        background-color: #2a213b;
        text-align: left;
    }
</style>