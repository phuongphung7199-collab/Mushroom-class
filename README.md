# Mushroom-class
Ok
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Flashcards</title> 
    <link rel="icon" type="image/png" href="https://lh3.googleusercontent.com/a/ACg8ocLm2zmxrsiuv3p344IkTBYO35_JmBkGlnUHW5wfOU9thQ07tSFz=s360-c-no">
<link rel="shortcut icon" type="image/png" href="https://lh3.googleusercontent.com/a/ACg8ocLm2zmxrsiuv3p344IkTBYO35_JmBkGlnUHW5wfOU9thQ07tSFz=s360-c-no">
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #111827;
            color: #e5e5e5;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .perspective-1000 { perspective: 1000px; }
        .transform-style-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
        .rotate-y-180 { transform: rotateY(180deg); }
        .card-container { transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1); }
        .card-flipped { transform: rotateY(180deg); }
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1f2937; }
        ::-webkit-scrollbar-thumb { background: #4b5563; border-radius: 4px; }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-4 bg-gray-900 overflow-hidden relative">
<div class="fixed top-4 left-4 z-50 flex gap-2">
        <button id="btn-export" class="w-10 h-10 bg-blue-600 hover:bg-blue-500 rounded-full text-white shadow-lg flex items-center justify-center transition active:scale-95" title="Xuất dữ liệu (Backup)">
            <i class="fas fa-file-export"></i>
        </button>
        <button id="btn-import" class="w-10 h-10 bg-purple-600 hover:bg-purple-500 rounded-full text-white shadow-lg flex items-center justify-center transition active:scale-95" title="Nhập dữ liệu (Restore)">
            <i class="fas fa-file-import"></i>
        </button>
        <input type="file" id="file-import" class="hidden" accept=".json">
    </div>
    <button id="btn-add-modal" class="fixed bottom-6 right-6 w-14 h-14 bg-green-600 hover:bg-green-500 rounded-full text-white shadow-2xl z-50 flex items-center justify-center transition-transform active:scale-90 border-2 border-green-400">
        <i class="fas fa-plus text-2xl"></i>
    </button>

    <div id="add-modal" class="hidden fixed inset-0 bg-black/80 z-[60] flex items-center justify-center p-4 backdrop-blur-sm">
        <div class="bg-gray-800 rounded-2xl w-full max-w-sm p-6 border border-gray-700 shadow-2xl relative">
            <h3 class="text-xl font-bold text-white mb-4">Thêm Flashcard mới</h3>
            <div class="mb-4">
                <label class="block text-gray-400 text-sm mb-1">Tiếng Anh (Word)</label>
                <input id="input-en" type="text" class="w-full bg-gray-900 border border-gray-600 rounded-lg p-3 text-white focus:border-green-500 outline-none" placeholder="Ví dụ: apple">
            </div>
            <div class="mb-6">
                <label class="block text-gray-400 text-sm mb-1">Nghĩa Tiếng Việt</label>
                <input id="input-vi" type="text" class="w-full bg-gray-900 border border-gray-600 rounded-lg p-3 text-white focus:border-green-500 outline-none" placeholder="Ví dụ: quả táo">
            </div>
            <div class="flex gap-3">
                <button id="btn-cancel" class="flex-1 py-3 bg-gray-700 hover:bg-gray-600 rounded-lg text-gray-300 font-medium">Hủy</button>
                <button id="btn-save" class="flex-1 py-3 bg-green-600 hover:bg-green-500 rounded-lg text-white font-bold shadow-lg flex justify-center items-center gap-2">
                    <span>Lưu</span>
                    <i id="spinner" class="fas fa-spinner fa-spin hidden"></i>
                </button>
            </div>
        </div>
    </div>

    <div class="w-full max-w-md mb-6 z-10 flex flex-col gap-4">
        <div class="flex justify-between items-end">
            <div>
                <h1 class="text-2xl font-bold text-blue-400 tracking-tight">Your <span class="text-white">Flashcards</span></h1>
                <p id="counter-text" class="text-xs text-gray-500 font-mono">Loading...</p>
            </div>
            <div id="index-display" class="text-right text-sm font-mono text-gray-300 bg-gray-800 px-3 py-1 rounded-full border border-gray-700">
                0 / 0
            </div>
        </div>
        <input id="search-input" type="text" placeholder="🔍 Tìm từ vựng (EN, VI)..." class="p-3 w-full bg-gray-800 rounded-lg text-white border border-gray-700 focus:border-blue-500 outline-none transition duration-200" />
    </div>

    <div id="flashcard-area" class="relative w-full max-w-md h-80 cursor-pointer perspective-1000 group z-10">
        <button id="btn-delete" class="absolute -top-10 right-0 w-8 h-8 text-gray-500 hover:text-red-500 transition z-50 flex items-center justify-center" title="Xóa flashcard này">
            <i class="fas fa-trash-alt text-lg"></i>
        </button>
        
        <div id="card-inner" class="relative w-full h-full duration-500 transform-style-3d card-container">
            <div class="absolute w-full h-full bg-gradient-to-br from-gray-800 to-gray-900 rounded-2xl shadow-2xl border-2 border-blue-500/50 p-8 flex flex-col items-center justify-center backface-hidden">
                <span class="absolute top-4 left-4 text-xs font-bold text-blue-400 tracking-widest bg-blue-500/10 px-2 py-1 rounded-md uppercase">English</span>
                <div class="flex-1 flex items-center justify-center w-full">
                    <h2 id="card-front-text" class="text-4xl md:text-5xl font-extrabold text-white text-center leading-tight break-words max-w-full">...</h2>
                </div>
                <button id="btn-speak-front" class="mt-4 w-12 h-12 rounded-full bg-blue-600 hover:bg-blue-500 flex items-center justify-center transition-all shadow-xl hover:shadow-blue-500/50 active:scale-95">
                    <i class="fas fa-volume-high text-white text-lg"></i>
                </button>
                <p class="mt-4 text-gray-500 text-xs flex items-center gap-2">
                    <i class="fas fa-hand-pointer"></i> Chạm hoặc Click để lật thẻ
                </p>
            </div>
            <div class="absolute w-full h-full bg-white rounded-2xl shadow-2xl p-8 flex flex-col items-center justify-center backface-hidden rotate-y-180 border-2 border-gray-300">
                <span class="absolute top-4 left-4 text-xs font-bold text-gray-600 tracking-widest uppercase border border-gray-300 px-2 py-1 rounded-md">Meaning</span>
                <div class="flex-1 flex flex-col items-center justify-center w-full space-y-4">
                    <h3 id="card-back-vi" class="text-3xl font-bold text-gray-900 text-center">...</h3>
                    <div class="px-4 py-2 bg-blue-50 rounded-lg border border-blue-100 shadow-inner">
                        <p id="card-back-ipa" class="text-xl text-blue-600 font-mono font-medium tracking-wide">...</p>
                    </div>
                    <p id="card-back-en" class="text-sm text-gray-400 font-semibold border-t border-gray-100 pt-2">...</p>
                </div>
                <button id="btn-speak-back" class="mb-2 w-10 h-10 rounded-full bg-gray-100 hover:bg-gray-200 text-gray-600 flex items-center justify-center transition-all">
                    <i class="fas fa-volume-high"></i>
                </button>
            </div>
        </div>
    </div>

    <div id="no-results" class="hidden text-center text-gray-400 text-xl mt-10 z-20">Không tìm thấy từ nào.</div>

    <div class="w-full max-w-md flex justify-between items-center mt-8 z-10 px-4">
        <button id="btn-prev" class="w-14 h-14 rounded-full bg-gray-800 border border-gray-700 text-gray-300 hover:text-white hover:border-gray-500 hover:bg-blue-800 transition flex items-center justify-center active:scale-95 shadow-lg">
            <i class="fas fa-arrow-left text-lg"></i>
        </button>
        <div class="text-xs text-gray-500 font-mono flex flex-col items-center">
            <span>← Prev | Flip (Space/Enter) | Next →</span>
            <span class="text-[10px] opacity-60">Use Arrow Keys on Desktop</span>
        </div>
        <button id="btn-next" class="w-14 h-14 rounded-full bg-gray-800 border border-gray-700 text-gray-300 hover:text-white hover:border-gray-500 hover:bg-blue-800 transition flex items-center justify-center active:scale-95 shadow-lg">
            <i class="fas fa-arrow-right text-lg"></i>
        </button>
    </div>

    <div class="absolute top-0 left-0 w-full h-full overflow-hidden -z-0 pointer-events-none opacity-20">
        <div class="absolute w-64 h-64 bg-blue-500 rounded-full blur-3xl opacity-30 -top-10 -left-10 animate-pulse"></div>
        <div class="absolute w-64 h-64 bg-purple-500 rounded-full blur-3xl opacity-20 bottom-0 right-0 animate-pulse animation-delay-5000"></div>
    </div>

    <script>
        // --- DATASET ---
        const defaultData = [
{ en: "feeding", vi: "cho ăn", ipa: "/ˈfiːdɪŋ/" },
{ en: "nursing", vi: "chăm sóc (mẹ/bé)", ipa: "/ˈnɜːrsɪŋ/" },
{ en: "breastfeeding", vi: "cho bú mẹ", ipa: "/ˈbrɛstˌfiːdɪŋ/" },
{ en: "wait", vi: "đợi", ipa: "/weɪt/" },
{ en: "listen", vi: "nghe", ipa: "/ˈlɪsən/" },
{ en: "look", vi: "nhìn", ipa: "/lʊk/" },
{ en: "watch", vi: "xem", ipa: "/wɑːtʃ/" },
        ];
// --- 2. LOGIC MỚI (ADD + LOCALSTORAGE + SWIPE + RANDOM) ---
        
        // Hàm xáo trộn mảng (Fisher-Yates Shuffle) - Đảo lộn vị trí ngẫu nhiên
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]]; // Hoán đổi vị trí
            }
            return array;
        }

        // Hàm Load dữ liệu: Gộp -> Sau đó Xáo trộn
        function loadFullData() {
            const stored = localStorage.getItem('my_flashcards');
            const personalData = stored ? JSON.parse(stored) : [];
            
            // Gộp 2 nguồn lại
            const combinedData = [...personalData, ...defaultData];
            
            // Trả về danh sách đã được xào bài
            return shuffleArray(combinedData);
        }

        // Khởi tạo biến chính
        let fullData = loadFullData(); 
        let filteredData = [...fullData];
        let currentIndex = 0;
        let isFlipped = false;

        // --- DOM ELEMENTS ---
        const els = {
            cardInner: document.getElementById('card-inner'),
            frontText: document.getElementById('card-front-text'),
            backVi: document.getElementById('card-back-vi'),
            backIpa: document.getElementById('card-back-ipa'),
            backEn: document.getElementById('card-back-en'),
            counter: document.getElementById('counter-text'),
            indexDisplay: document.getElementById('index-display'),
            searchInput: document.getElementById('search-input'),
            btnPrev: document.getElementById('btn-prev'),
            btnNext: document.getElementById('btn-next'),
            btnSpeakFront: document.getElementById('btn-speak-front'),
            btnSpeakBack: document.getElementById('btn-speak-back'),
            flashcardArea: document.getElementById('flashcard-area'),
            noResults: document.getElementById('no-results'),
            addModal: document.getElementById('add-modal'),
            btnAddModal: document.getElementById('btn-add-modal'),
            btnCancel: document.getElementById('btn-cancel'),
            btnSave: document.getElementById('btn-save'),
            inputEn: document.getElementById('input-en'),
            inputVi: document.getElementById('input-vi'),
            spinner: document.getElementById('spinner'),
            btnDelete: document.getElementById('btn-delete'),
            btnExport: document.getElementById('btn-export'),
            btnImport: document.getElementById('btn-import'),
            fileImport: document.getElementById('file-import')
        };

        // --- RENDER & LOGIC CARD ---
        function renderCard() {
            if (filteredData.length === 0) {
                els.flashcardArea.style.display = 'none';
                els.noResults.classList.remove('hidden');
                els.indexDisplay.textContent = "0 / 0";
                els.counter.textContent = "No words found";
                return;
            }

            els.flashcardArea.style.display = 'block';
            els.noResults.classList.add('hidden');

            const card = filteredData[currentIndex];
            els.frontText.textContent = card.en;
            els.backVi.textContent = card.vi;
            els.backIpa.textContent = card.ipa || "/.../"; 
            els.backEn.textContent = card.en;

            els.indexDisplay.textContent = `${currentIndex + 1} / ${filteredData.length}`;
            els.counter.textContent = filteredData.length === fullData.length 
                ? `Tổng: ${fullData.length} thẻ (Randomized)`
                : `Found: ${filteredData.length} / ${fullData.length}`;
            
            els.btnPrev.disabled = filteredData.length <= 1;
            els.btnNext.disabled = filteredData.length <= 1;
            els.btnPrev.style.opacity = filteredData.length <= 1 ? "0.3" : "1";
            els.btnNext.style.opacity = filteredData.length <= 1 ? "0.3" : "1";
        }

        function flipCard() {
            isFlipped = !isFlipped;
            els.cardInner.classList.toggle('card-flipped', isFlipped);
        }

        function nextCard() {
            if (filteredData.length <= 1) return;
            isFlipped = false;
            els.cardInner.classList.remove('card-flipped');
            setTimeout(() => {
                currentIndex = (currentIndex + 1) % filteredData.length;
                renderCard();
            }, 150);
        }

        function prevCard() {
            if (filteredData.length <= 1) return;
            isFlipped = false;
            els.cardInner.classList.remove('card-flipped');
            setTimeout(() => {
                currentIndex = (currentIndex - 1 + filteredData.length) % filteredData.length;
                renderCard();
            }, 150);
        }

        function speak(text) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'en-US';
                utterance.rate = 0.9;
                window.speechSynthesis.speak(utterance);
            }
        }

        // --- EVENT LISTENERS ---
        
        els.btnAddModal.addEventListener('click', () => {
            els.addModal.classList.remove('hidden');
            els.inputEn.focus();
        });
        
        els.btnCancel.addEventListener('click', () => {
            els.addModal.classList.add('hidden');
            els.inputEn.value = '';
            els.inputVi.value = '';
        });

        async function fetchIPA(word) {
            try {
                const res = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
                const data = await res.json();
                if (Array.isArray(data) && data.length > 0) {
                    return data[0].phonetic || (data[0].phonetics.find(p => p.text)?.text) || "";
                }
                return "";
            } catch (err) {
                console.error("Lỗi lấy IPA:", err);
                return ""; 
            }
        }

        els.btnSave.addEventListener('click', async () => {
            const en = els.inputEn.value.trim();
            const vi = els.inputVi.value.trim();

            if (!en || !vi) {
                alert("Vui lòng nhập đủ thông tin!");
                return;
            }

            // Kiểm tra trùng
            const isDuplicate = fullData.some(card => card.en.toLowerCase() === en.toLowerCase());
            if (isDuplicate) {
                const userConfirmed = confirm(`Từ "${en}" đã có trong danh sách rồi. Mày có chắc chắn muốn thêm thẻ mới này không?`);
                if (!userConfirmed) return;
            }

            els.btnSave.disabled = true;
            els.spinner.classList.remove('hidden');

            let ipa = await fetchIPA(en);
            if (!ipa) ipa = "/.../"; 

            const newCard = { en, vi, ipa };

            const stored = localStorage.getItem('my_flashcards');
            const personalData = stored ? JSON.parse(stored) : [];
            personalData.unshift(newCard); 
            localStorage.setItem('my_flashcards', JSON.stringify(personalData));

            // Reload và Shuffle lại
            fullData = loadFullData(); 
            filteredData = [...fullData];
            
            // Logic đặc biệt: Vì vừa shuffle nên từ mới thêm có thể bay đi đâu đó.
            // Tao sẽ tìm vị trí của nó để hiển thị ngay cho mày xem.
            const newIndex = fullData.findIndex(item => item.en === en && item.vi === vi);
            currentIndex = newIndex !== -1 ? newIndex : 0;

            els.inputEn.value = '';
            els.inputVi.value = '';
            els.addModal.classList.add('hidden');
            
            renderCard();
            
            els.btnSave.disabled = false;
            els.spinner.classList.add('hidden');
        });

        els.btnDelete.addEventListener('click', (e) => {
            e.stopPropagation(); 

            const currentCard = filteredData[currentIndex];
            const stored = localStorage.getItem('my_flashcards');
            let personalData = stored ? JSON.parse(stored) : [];

            const indexToDelete = personalData.findIndex(item => 
                item.en === currentCard.en && item.vi === currentCard.vi
            );

            if (indexToDelete !== -1) {
                const confirmDelete = confirm(`Mày muốn xóa từ "${currentCard.en}" vĩnh viễn chứ?`);
                if (confirmDelete) {
                    personalData.splice(indexToDelete, 1);
                    localStorage.setItem('my_flashcards', JSON.stringify(personalData));

                    fullData = loadFullData(); // Reload và shuffle lại
                    const term = els.searchInput.value.toLowerCase().trim();
                    filteredData = fullData.filter(item => 
                        item.en.toLowerCase().includes(term) || 
                        item.vi.toLowerCase().includes(term)
                    );

                    if (currentIndex >= filteredData.length) {
                        currentIndex = Math.max(0, filteredData.length - 1);
                    }
                    
                    renderCard();
                }
            } else {
                alert("Đây là thẻ mặc định của hệ thống, không xóa được đâu nha!");
            }
        });

        els.searchInput.addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase().trim();
            filteredData = fullData.filter(item => 
                item.en.toLowerCase().includes(term) || 
                item.vi.toLowerCase().includes(term)
            );
            currentIndex = 0;
            isFlipped = false;
            els.cardInner.classList.remove('card-flipped');
            renderCard();
        });

        els.flashcardArea.addEventListener('click', flipCard);
        els.btnNext.addEventListener('click', nextCard);
        els.btnPrev.addEventListener('click', prevCard);

        els.btnSpeakFront.addEventListener('click', (e) => { e.stopPropagation(); speak(filteredData[currentIndex].en); });
        els.btnSpeakBack.addEventListener('click', (e) => { e.stopPropagation(); speak(filteredData[currentIndex].en); });

        let touchStartX = 0;
        let touchStartY = 0;
        const minSwipeDistance = 50;
        document.addEventListener('touchstart', (e) => {
            if (e.target.closest('#add-modal')) return;
            touchStartX = e.changedTouches[0].screenX;
            touchStartY = e.changedTouches[0].screenY;
        }, { passive: true });

        document.addEventListener('touchend', (e) => {
            if (e.target.closest('#add-modal')) return;
            const touchEndX = e.changedTouches[0].screenX;
            const touchEndY = e.changedTouches[0].screenY;
            const dX = touchStartX - touchEndX;
            const dY = touchStartY - touchEndY;
            if (Math.abs(dX) > Math.abs(dY) && Math.abs(dX) > minSwipeDistance) {
                dX > 0 ? nextCard() : prevCard();
            }
        }, { passive: true });

        window.addEventListener('keydown', (e) => {
            if (document.activeElement === els.searchInput || !els.addModal.classList.contains('hidden')) return;
            if (e.key === 'ArrowRight') nextCard();
            if (e.key === 'ArrowLeft') prevCard();
            if (e.key === ' ' || e.key === 'Enter') { e.preventDefault(); flipCard(); }
        });

        // --- LOGIC EXPORT (XUẤT FILE) ---
        els.btnExport.addEventListener('click', () => {
            const stored = localStorage.getItem('my_flashcards');
            const dataStr = stored || "[]"; // Lấy dữ liệu cá nhân
            
            // Tạo file ảo để tải về
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            const exportFileDefaultName = 'my_flashcards_backup.json';

            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click(); // Tự động bấm tải
        });

        // --- LOGIC IMPORT (NHẬP FILE) ---
        // 1. Bấm nút Import -> Kích hoạt thẻ input file ẩn
        els.btnImport.addEventListener('click', () => {
            els.fileImport.click();
        });

        // 2. Khi chọn file xong -> Xử lý
        els.fileImport.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (event) => {
                try {
                    const importedData = JSON.parse(event.target.result);
                    
                    if (!Array.isArray(importedData)) {
                        alert("File lỗi! Không đúng định dạng.");
                        return;
                    }

                    // Lấy dữ liệu hiện tại
                    const stored = localStorage.getItem('my_flashcards');
                    let currentData = stored ? JSON.parse(stored) : [];

                    // Gộp dữ liệu (Bỏ qua từ trùng)
                    let countAdded = 0;
                    importedData.forEach(newCard => {	
                        const exists = currentData.some(c => c.en.toLowerCase() === newCard.en.toLowerCase());
                        if (!exists) {
                            currentData.unshift(newCard); // Thêm từ mới lên đầu
                            countAdded++;
                        }
                    });

                    // Lưu ngược lại vào bộ nhớ
                    localStorage.setItem('my_flashcards', JSON.stringify(currentData));

                    // Reload lại app
                    fullData = loadFullData();
                    filteredData = [...fullData];
                    currentIndex = 0;
                    renderCard();

                    alert(`Thành công! Đã nhập thêm ${countAdded} từ mới.`);
                } catch (err) {
                    console.error(err);
                    alert("Lỗi đọc file json!");
                }
            };
            reader.readAsText(file);
            e.target.value = ''; // Reset input để chọn lại file cũ được
        });

        renderCard();

    </script>
</body>
</html>

