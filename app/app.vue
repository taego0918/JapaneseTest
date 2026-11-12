<template>
    <div style="background-color: black; min-height: 100vh; padding: 20px;">
        <header style="text-align: center; margin-bottom: 20px;">
            <select v-model="level" class="level-select">
                <option value="N4HiRaGaNa">N4-讀音</option>
                <option value="N5">N5 題庫</option>
            </select>
        </header>

        <NuxtPage />
    </div>
</template>

<script setup>
    const level = useState('quizLevel', () => 'N4HiRaGaNa');
</script>

<style>
    body {
        background-color: black;
        margin: 0;
    }

    /* === 針對 Firefox 的捲軸樣式 === */
    html {
        scrollbar-width: thin;
        scrollbar-color: #353848 #121216; /* 滑塊顏色 與 軌道顏色 */
    }

    /* === 針對 Chrome, Safari, Edge 的捲軸樣式 === */
    ::-webkit-scrollbar {
        width: 8px; /* 捲軸寬度 */
    }

    ::-webkit-scrollbar-track {
        background: #121216; /* 捲軸軌道背景色（配合你的暗色主題） */
    }

    ::-webkit-scrollbar-thumb {
        background: #353848; /* 捲軸滑塊顏色（呼應你的選項邊框色） */
        border-radius: 4px;  /* 滑塊圓角 */
    }

    ::-webkit-scrollbar-thumb:hover {
        background: #4a4e63; /* 滑鼠懸停時滑塊的顏色變亮一點 */
    }

    .level-select {
        padding: 8px 16px;
        cursor: pointer;
        color: white;
        background-color: #252530;
        border: 1px solid #353848;
        border-radius: 4px;
        font-size: 16px;
    }
</style>