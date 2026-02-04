// 题目数据
const questions = [
    {
        id: 1,
        sentence: "Despite the heavy rain, the football match continued as scheduled.",
        words: ["Despite", "the", "heavy", "rain,", "the", "football", "match", "continued", "as", "scheduled."]
    },
    {
        id: 2,
        sentence: "The book that I borrowed from the library was extremely informative.",
        words: ["The", "book", "that", "I", "borrowed", "from", "the", "library", "was", "extremely", "informative."]
    },
    {
        id: 3,
        sentence: "If I were you, I would take that job offer immediately.",
        words: ["If", "I", "were", "you,", "I", "would", "take", "that", "job", "offer", "immediately."]
    },
    {
        id: 4,
        sentence: "Not only did she finish her work, but she also helped others.",
        words: ["Not", "only", "did", "she", "finish", "her", "work,", "but", "she", "also", "helped", "others."]
    },
    {
        id: 5,
        sentence: "Having completed the project ahead of schedule, the team celebrated their success.",
        words: ["Having", "completed", "the", "project", "ahead", "of", "schedule,", "the", "team", "celebrated", "their", "success."]
    }
];

// 游戏状态
let currentQuestion = 0;
let score = 0;
let userAnswer = Array(questions[currentQuestion].words.length).fill(null);
let correctOrder = [];

// DOM元素
const wordBank = document.getElementById('word-bank');
const dropZones = document.querySelectorAll('.drop-zone');
const targetSentence = document.getElementById('target-sentence');
const scoreElement = document.getElementById('score');
const progressElement = document.getElementById('progress');
const feedback = document.getElementById('feedback');
const feedbackText = document.getElementById('feedback-text');
const checkBtn = document.getElementById('check-btn');
const nextBtn = document.getElementById('next-btn');
const hintBtn = document.getElementById('hint-btn');
const resetBtn = document.getElementById('reset-btn');
const showAnswerBtn = document.getElementById('show-answer-btn');
const questionBtns = document.querySelectorAll('.question-btn');

// 初始化游戏
function initGame() {
    const question = questions[currentQuestion];
    
    // 更新界面
    targetSentence.textContent = question.sentence;
    progressElement.textContent = currentQuestion + 1;
    
    // 清空单词库和放置区域
    wordBank.innerHTML = '';
    dropZones.forEach(zone => {
        zone.innerHTML = '';
        zone.classList.remove('filled', 'hover');
    });
    
    // 重置答案数组
    correctOrder = [...question.words];
    userAnswer = Array(question.words.length).fill(null);
    
    // 创建可拖拽单词
    shuffleArray([...question.words]).forEach(word => {
        const wordElement = document.createElement('div');
        wordElement.className = 'word';
        wordElement.textContent = word;
        wordElement.draggable = true;
        wordElement.dataset.word = word;
        
        // 拖拽事件
        wordElement.addEventListener('dragstart', handleDragStart);
        wordElement.addEventListener('dragend', handleDragEnd);
        
        wordBank.appendChild(wordElement);
    });
    
    // 更新放置区域数量
    updateDropZones();
    
    // 重置反馈
    feedback.className = 'hidden';
    nextBtn.disabled = true;
}

// Fisher-Yates洗牌算法
function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
}

// 更新放置区域
function updateDropZones() {
    dropZones.forEach((zone, index) => {
        zone.innerHTML = userAnswer[index] || '';
        zone.classList.toggle('filled', userAnswer[index] !== null);
    });
}

// 拖拽开始
function handleDragStart(e) {
    e.dataTransfer.setData('text/plain', e.target.dataset.word);
    e.target.classList.add('dragging');
    
    // 添加拖拽效果到所有放置区域
    dropZones.forEach(zone => zone.classList.add('hover'));
}

// 拖拽结束
function handleDragEnd(e) {
    e.target.classList.remove('dragging');
    dropZones.forEach(zone => zone.classList.remove('hover'));
}

// 放置区域事件
dropZones.forEach(zone => {
    zone.addEventListener('dragover', e => {
        e.preventDefault();
        zone.classList.add('hover');
    });
    
    zone.addEventListener('dragleave', () => {
        zone.classList.remove('hover');
    });
    
    zone.addEventListener('drop', e => {
        e.preventDefault();
        zone.classList.remove('hover');
        
        const word = e.dataTransfer.getData('text/plain');
        const index = parseInt(zone.dataset.id) - 1;
        
        // 如果该位置已有单词，放回单词库
        if (userAnswer[index]) {
            returnWordToBank(userAnswer[index]);
        }
        
        // 放置单词
        userAnswer[index] = word;
        updateDropZones();
        
        // 从单词库移除已使用的单词
        removeWordFromBank(word);
        
        // 播放音效
        playSound('drop');
    });
});

// 从单词库移除单词
function removeWordFromBank(word) {
    const words = document.querySelectorAll('.word');
    words.forEach(w => {
        if (w.dataset.word === word && !w.classList.contains('used')) {
            w.classList.add('used');
            w.draggable = false;
        }
    });
}

// 将单词放回单词库
function returnWordToBank(word) {
    const words = document.querySelectorAll('.word');
    words.forEach(w => {
        if (w.dataset.word === word && w.classList.contains('used')) {
            w.classList.remove('used');
            w.draggable = true;
        }
    });
}

// 检查答案
checkBtn.addEventListener('click', () => {
    const question = questions[currentQuestion];
    let isCorrect = true;
    
    // 检查每个位置
    for (let i = 0; i < question.words.length; i++) {
        if (userAnswer[i] !== question.words[i]) {
            isCorrect = false;
            break;
        }
    }
    
    // 显示反馈
    if (isCorrect) {
        score += 10;
        scoreElement.textContent = score;
        
        feedback.className = 'correct pulse';
        feedbackText.textContent = '🎉 太棒了！完全正确！';
        feedback.classList.remove('hidden');
        
        playSound('correct');
        
        // 启用下一题按钮
        nextBtn.disabled = false;
        
        // 标记所有单词为已使用
        document.querySelectorAll('.word').forEach(w => w.classList.add('used'));
    } else {
        feedback.className = 'incorrect shake';
        feedbackText.textContent = '😅 再试一次！有些单词位置不对。';
        feedback.classList.remove('hidden');
        
        playSound('incorrect');
    }
});

// 下一题
nextBtn.addEventListener('click', () => {
    currentQuestion++;
    
    if (currentQuestion >= questions.length) {
        // 游戏结束
        alert(`恭喜！游戏结束！\n你的最终得分: ${score}\n正确率: ${(score / (questions.length * 10) * 100).toFixed(1)}%`);
        currentQuestion = 0;
        score = 0;
        scoreElement.textContent = score;
    }
    
    // 更新题目按钮状态
    questionBtns.forEach(btn => {
        btn.classList.toggle('active', parseInt(btn.dataset.id) === currentQuestion + 1);
    });
    
    initGame();
});

// 提示
hintBtn.addEventListener('click', () => {
    const question = questions[currentQuestion];
    
    // 找出第一个错误的空位
    let emptyIndex = -1;
    for (let i = 0; i < question.words.length; i++) {
        if (!userAnswer[i]) {
            emptyIndex = i;
            break;
        }
    }
    
    if (emptyIndex !== -1) {
        // 填充一个正确单词
        const correctWord = question.words[emptyIndex];
        userAnswer[emptyIndex] = correctWord;
        updateDropZones();
        removeWordFromBank(correctWord);
        
        // 闪烁提示
        dropZones[emptyIndex].classList.add('pulse');
        setTimeout(() => dropZones[emptyIndex].classList.remove('pulse'), 1000);
        
        feedback.className = 'correct';
        feedbackText.textContent = `💡 提示：第${emptyIndex + 1}个位置应该是"${correctWord}"`;
        feedback.classList.remove('hidden');
        
        playSound('hint');
    }
});

// 重置当前题目
resetBtn.addEventListener('click', () => {
    initGame();
    playSound('reset');
});

// 显示答案
showAnswerBtn.addEventListener('click', () => {
    const question = questions[currentQuestion];
    
    // 填充所有正确答案
    question.words.forEach((word, index) => {
        userAnswer[index] = word;
    });
    
    updateDropZones();
    
    // 标记所有单词为已使用
    document.querySelectorAll('.word').forEach(w => w.classList.add('used'));
    
    feedback.className = 'correct';
    feedbackText.textContent = '📖 答案已显示。学习并记住这个句子结构！';
    feedback.classList.remove('hidden');
    
    playSound('show');
});

// 选择题目
questionBtns.forEach(btn => {
    btn.addEventListener('click', () => {
        const questionId = parseInt(btn.dataset.id);
        currentQuestion = questionId - 1;
        
        // 更新按钮状态
        questionBtns.forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        
        initGame();
        playSound('click');
    });
});

// 音效
function playSound(type) {
    // 实际项目中可以使用真实的音效文件
    const sounds = {
        correct: { frequency: 523.25, duration: 300 }, // C5
        incorrect: { frequency: 349.23, duration: 300 }, // F4
        drop: { frequency: 392.00, duration: 100 }, // G4
        hint: { frequency: 659.25, duration: 200 }, // E5
        reset: { frequency: 293.66, duration: 200 }, // D4
        show: { frequency: 440.00, duration: 300 }, // A4
        click: { frequency: 261.63, duration: 50 } // C4
    };
    
    if (sounds[type]) {
        // 使用Web Audio API播放简单音效
        try {
            const audioContext = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioContext.createOscillator();
            const gainNode = audioContext.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioContext.destination);
            
            oscillator.frequency.value = sounds[type].frequency;
            oscillator.type = 'sine';
            
            gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + sounds[type].duration / 1000);
            
            oscillator.start(audioContext.currentTime);
            oscillator.stop(audioContext.currentTime + sounds[type].duration / 1000);
        } catch (e) {
            console.log('音效播放失败:', e);
        }
    }
}

// 初始化游戏
initGame();

// 添加键盘快捷键
document.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !nextBtn.disabled) {
        nextBtn.click();
    } else if (e.key === ' ' || e.key === 'Spacebar') {
        e.preventDefault();
        checkBtn.click();
    } else if (e.key === 'h' || e.key === 'H') {
        hintBtn.click();
    } else if (e.key === 'r' || e.key === 'R') {
        resetBtn.click();
    } else if (e.key === 'a' || e.key === 'A') {
        showAnswerBtn.click();
    } else if (e.key >= '1' && e.key <= '5') {
        const index = parseInt(e.key) - 1;
        if (questionBtns[index]) {
            questionBtns[index].click();
        }
    }
});

// 显示键盘快捷键提示
console.log(`
🎮 键盘快捷键：
空格键 - 检查答案
回车键 - 下一题
H - 提示
R - 重置当前题目
A - 显示答案
1-5 - 选择题目
`);
