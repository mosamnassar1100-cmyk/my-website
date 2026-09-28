// قائمة الـ 21 عصابة
const gangsData = [
    { name: "Grove Street", emoji: "🟣", color: "#a855f7", glow: "rgba(168, 85, 247, 0.25)" },
    { name: "Vagos", emoji: "🟡", color: "#eab308", glow: "rgba(234, 179, 8, 0.25)" },
    { name: "Scrap", emoji: "🟤", color: "#92400e", glow: "rgba(146, 64, 14, 0.25)" },
    { name: "Families", emoji: "🟢", color: "#22c55e", glow: "rgba(34, 197, 94, 0.25)" },
    { name: "Quietless", emoji: "🏴‍☠️", color: "#64748b", glow: "rgba(100, 116, 139, 0.25)" },
    { name: "Red Reaper", emoji: "🔴", color: "#ef4444", glow: "rgba(239, 68, 68, 0.25)" },
    { name: "Death Phantoms", emoji: "🏳️", color: "#cbd5e1", glow: "rgba(203, 213, 225, 0.25)" },
    { name: "N.W.A", emoji: "💀", color: "#475569", glow: "rgba(71, 85, 105, 0.25)" },
    { name: "Ninety Nine", emoji: "🕷️", color: "#94a3b8", glow: "rgba(148, 163, 184, 0.25)" },
    { name: "chesy", emoji: "🧀", color: "#f59e0b", glow: "rgba(245, 158, 11, 0.25)" },
    { name: "Twenty Two", emoji: "🦅", color: "#78716c", glow: "rgba(120, 113, 108, 0.25)" },
    { name: "Dark Side", emoji: "🔷", color: "#3b82f6", glow: "rgba(59, 130, 246, 0.25)" },
    { name: "Nightmare", emoji: "🔥", color: "#f97316", glow: "rgba(249, 115, 22, 0.25)" },
    { name: "El Parton", emoji: "♠️", color: "#334155", glow: "rgba(51, 65, 85, 0.25)" },
    { name: "black shadow", emoji: "🪶", color: "#e2e8f0", glow: "rgba(226, 232, 240, 0.25)" },
    { name: "Red Storm", emoji: "🪓", color: "#dc2626", glow: "rgba(220, 38, 38, 0.25)" },
    { name: "Old School", emoji: "⚪", color: "#ffffff", glow: "rgba(255, 255, 255, 0.25)" },
    { name: "Trickster", emoji: "⚡", color: "#eab308", glow: "rgba(234, 179, 8, 0.25)" },
    { name: "Last Call", emoji: "📞", color: "#6b7280", glow: "rgba(107, 114, 128, 0.25)" },
    { name: "Darkness", emoji: "🔘", color: "#1e293b", glow: "rgba(30, 41, 59, 0.25)" },
    { name: "Sons", emoji: "🟠", color: "#f97316", glow: "rgba(249, 115, 22, 0.25)" },
    { name: "Red Wedding", emoji: "🚩", color: "#b91c1c", glow: "rgba(185, 28, 28, 0.25)" }
];

let currentActiveGangKey = "";

// تعبئة كامل مساحة الهيدر بأكواد الهكر المتحركة لتغطي الخلفية بالكامل
function initMatrixBackground() {
    const matrixBg = document.getElementById("matrix-bg");
    const chars = "01FBI-SECURE-VAGOS-GROVE-DATA-ACCESS-99#@!SEC-OPS-ROOT-SYSTEM-ONLINE-TARGET-LOCKED-OPERATION";
    
    setInterval(() => {
        let text = "";
        for (let i = 0; i < 350; i++) {
            text += chars.charAt(Math.floor(Math.random() * chars.length)) + " ";
            if (i % 28 === 0) text += "\n";
        }
        matrixBg.textContent = text;
    }, 90);
}

// أكواد الهكر لقسم المواطنين الفسدة في نهاية الصفحة
function initCitizenFinalMatrix() {
    const matrixBg = document.getElementById("citizen-final-matrix");
    if (!matrixBg) return;
    const chars = "01CITIZENS-CORRUPT-DATA-SECURE-ACCESS-OPS-SYSTEM-ONLINE-RECORD-LOCKED";
    setInterval(() => {
        let text = "";
        for (let i = 0; i < 350; i++) {
            text += chars.charAt(Math.floor(Math.random() * chars.length)) + " ";
            if (i % 28 === 0) text += "\n";
        }
        matrixBg.textContent = text;
    }, 90);
}

// توليد الكارتات
document.addEventListener("DOMContentLoaded", () => {
    initMatrixBackground();
    initCitizenFinalMatrix();
    const gridContainer = document.getElementById("gangs-grid-container");
    
    gangsData.forEach((gang) => {
        const card = document.createElement("div");
        card.className = "gang-card";
        card.style.setProperty('--gang-color', gang.color);
        card.style.setProperty('--gang-glow', gang.glow);
        card.setAttribute("data-name", gang.name.toLowerCase());

        card.innerHTML = `
            <div>
                <div class="gang-emoji">${gang.emoji}</div>
                <h3>${gang.name}</h3>
                <div class="gang-status">ملف الأمان: نشط تحت المراقبة</div>
            </div>
            <button onclick="openModal('${gang.name}', '${gang.emoji}')" class="gang-open-btn">عرض الملف</button>
        `;
        gridContainer.appendChild(card);
    });
});

// شريط البحث المباشر
function filterGangs() {
    const query = document.getElementById("gang-search").value.toLowerCase();
    const cards = document.querySelectorAll(".gang-card");

    cards.forEach(card => {
        const name = card.getAttribute("data-name");
        if (name.includes(query)) {
            card.style.display = "flex";
        } else {
            card.style.display = "none";
        }
    });
}

// فتح نافذة العصابة وتحميل بياناتها المنفصلة تماماً
function openModal(gangName, emoji) {
    currentActiveGangKey = "fbi_gang_" + gangName.replace(/\s+/g, '_').toLowerCase();
    
    document.getElementById("modal-gang-title").textContent = `ملف التحقيق: ${emoji} ${gangName}`;
    
    const savedData = JSON.parse(localStorage.getItem(currentActiveGangKey)) || {
        notes: `تقرير العمليات الخاص بعصابة [${gangName}]:\n- حالة المراقبة الميدانية: نشطة.\n- مستوى التهديد الأمني: قيد المراجعة.`,
        images: []
    };

    document.getElementById("gang-smart-notes").value = savedData.notes;
    
    const imagesGridContainer = document.getElementById("images-grid-container");
    imagesGridContainer.innerHTML = "";
    
    savedData.images.forEach(imgData => {
        appendImageCard(imgData.src, imgData.title, imagesGridContainer);
    });

    document.getElementById("pro-modal").classList.add("active");
}

function closeModal() {
    document.getElementById("pro-modal").classList.remove("active");
}

function handleImages(event) {
    const files = event.target.files;
    const imagesGridContainer = document.getElementById("images-grid-container");
    
    if (files && files.length > 0) {
        Array.from(files).forEach(file => {
            const reader = new FileReader();
            reader.onload = function(e) {
                appendImageCard(e.target.result, "", imagesGridContainer);
            }
            reader.readAsDataURL(file);
        });
        event.target.value = "";
    }
}

function appendImageCard(src, titleText, container) {
    const itemDiv = document.createElement("div");
    itemDiv.className = "image-card-item";
    
    const img = document.createElement("img");
    img.src = src;
    img.alt = titleText || "الصورة المرفوعة";
    img.addEventListener("click", () => openImageLightbox(src));

    const infoBox = document.createElement("div");
    infoBox.className = "image-info-box";

    const label = document.createElement("label");
    label.textContent = "عنوان أو وصف الصورة:";

    const titleInput = document.createElement("input");
    titleInput.type = "text";
    titleInput.className = "image-title-input";
    titleInput.value = titleText || "";
    titleInput.placeholder = "اكتب عنوان الدليل هنا...";

    infoBox.appendChild(label);
    infoBox.appendChild(titleInput);

    const deleteBtn = document.createElement("button");
    deleteBtn.className = "delete-img-btn";
    deleteBtn.innerHTML = "&times;";
    deleteBtn.title = "حذف الصورة";
    
    deleteBtn.addEventListener("click", () => {
        itemDiv.remove();
    });

    itemDiv.appendChild(img);
    itemDiv.appendChild(infoBox);
    itemDiv.appendChild(deleteBtn);
    
    container.appendChild(itemDiv);
}


function openImageLightbox(src) {
    const lightbox = document.getElementById("image-lightbox");
    const lightboxImage = document.getElementById("lightbox-image");
    if (!lightbox || !lightboxImage) return;

    lightboxImage.src = src;
    lightbox.classList.add("active");
    document.body.style.overflow = "hidden";
}

function closeImageLightbox(event) {
    if (event) event.stopPropagation();

    const lightbox = document.getElementById("image-lightbox");
    const lightboxImage = document.getElementById("lightbox-image");
    if (!lightbox) return;

    lightbox.classList.remove("active");
    if (lightboxImage) lightboxImage.src = "";
    document.body.style.overflow = "";
}

document.addEventListener("keydown", (event) => {
    if (event.key === "Escape") {
        closeImageLightbox();
    }
});

function saveCurrentGangData(btn) {
    if (!currentActiveGangKey) return;

    const notes = document.getElementById("gang-smart-notes").value;
    const imageItems = document.querySelectorAll(".image-card-item");
    const imagesArray = [];

    imageItems.forEach(item => {
        const imgSrc = item.querySelector("img").src;
        const imgTitle = item.querySelector(".image-title-input").value;
        imagesArray.push({ src: imgSrc, title: imgTitle });
    });

    const dataToSave = {
        notes: notes,
        images: imagesArray
    };

    localStorage.setItem(currentActiveGangKey, JSON.stringify(dataToSave));

    const originalText = btn.textContent;
    btn.textContent = "جاري الحفظ في الأرشيف المركزي... ⏳";
    btn.style.background = "#2563eb";
    
    setTimeout(() => {
        btn.textContent = "تم الحفظ بنجاح لهذه العصابة ✓";
        btn.style.background = "#22c55e";
        setTimeout(() => {
            btn.textContent = originalText;
            btn.style.background = "#3b82f6";
            closeModal();
        }, 1000);
    }, 1000);
}
