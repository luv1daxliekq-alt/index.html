<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DEFY STORE - ร้านค้าออนไลน์ & ระบบรีวิว (Dark Theme)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font: Kanit -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Kanit', 'sans-serif'],
                    },
                    colors: {
                        dark: {
                            950: '#050505',
                            900: '#0c0c0e',
                            800: '#141417',
                            700: '#1e1e24',
                            600: '#2a2a33',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Kanit', sans-serif;
            background-color: #050505;
            color: #f4f4f5;
            overflow-x: hidden;
        }

        /* Animated Background Canvas for Smoke/Fog & Continuous Snow */
        #bgCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            pointer-events: none;
            z-index: 1;
        }

        /* Black Aura Effect */
        .black-aura {
            box-shadow: 0 0 50px rgba(0, 0, 0, 0.95), 0 0 100px rgba(20, 20, 20, 0.8);
        }
        
        .white-glow {
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.15);
        }

        .white-glow-hover:hover {
            box-shadow: 0 0 25px rgba(255, 255, 255, 0.25);
            border-color: rgba(255, 255, 255, 0.4);
        }

        /* Aspect ratio 16:9 banner wrapper */
        .aspect-banner {
            aspect-ratio: 16 / 9;
        }

        /* Glassmorphic dark card styling */
        .glass-card {
            background: rgba(18, 18, 22, 0.85);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* Scrollbar customization */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #050505;
        }
        ::-webkit-scrollbar-thumb {
            background: #27272a;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #3f3f46;
        }
    </style>
</head>
<body class="bg-black text-zinc-100 min-h-screen pb-16 relative">

    <!-- Background Canvas for Smoke/Fog & Snow Particle Animations -->
    <canvas id="bgCanvas"></canvas>

    <!-- Navigation Bar -->
    <nav class="bg-zinc-950/90 border-b border-zinc-800/80 sticky top-0 z-30 backdrop-blur-md shadow-2xl">
        <div class="max-w-4xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="w-9 h-9 rounded-xl bg-white text-black flex items-center justify-center font-bold text-lg shadow-lg shadow-white/10">
                    <i class="fa-solid fa-store text-base"></i>
                </div>
                <span id="navStoreName" class="font-bold text-white tracking-wide text-lg">DEFY STORE</span>
            </div>
            
            <button onclick="toggleAdminAuthModal()" id="adminToggleBtn" class="flex items-center space-x-2 px-3.5 py-1.5 text-xs font-semibold rounded-full bg-zinc-900 border border-zinc-700 hover:bg-zinc-800 text-zinc-300 transition duration-300 white-glow-hover">
                <i class="fa-solid fa-user-shield text-zinc-400"></i>
                <span id="adminBtnText">โหมดหลังบ้าน (Admin)</span>
            </button>
        </div>
    </nav>

    <!-- Main Container -->
    <main class="max-w-4xl mx-auto px-4 pt-6 relative z-10">
        
        <!-- Profile Banner Card with Black Aura -->
        <div class="glass-card rounded-3xl shadow-2xl overflow-hidden mb-8 black-aura relative">
            <!-- Dark Aura Radial Overlay -->
            <div class="absolute inset-0 bg-gradient-to-b from-transparent via-black/40 to-black pointer-events-none z-10"></div>

            <!-- Cover Photo -->
            <div class="relative h-52 sm:h-72 bg-zinc-900 overflow-hidden">
                <img id="coverImg" src="https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=1200&auto=format&fit=crop" 
                     alt="Cover Photo" class="w-full h-full object-cover filter brightness-75 contrast-110">
                <div class="absolute inset-0 bg-gradient-to-t from-black via-black/40 to-transparent"></div>
            </div>

            <!-- Profile Info Section -->
            <div class="relative px-6 pb-6 pt-0 z-20">
                <div class="flex flex-col sm:flex-row sm:items-end justify-between -mt-16 sm:-mt-20 mb-4 gap-4">
                    <!-- Profile Avatar -->
                    <div class="relative">
                        <img id="profileImg" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=300&auto=format&fit=crop" 
                             alt="Profile Avatar" class="w-28 h-28 sm:w-36 sm:h-36 rounded-2xl border-2 border-zinc-700 object-cover shadow-2xl bg-zinc-900 ring-4 ring-black">
                        <span class="absolute bottom-2 right-2 w-4 h-4 bg-emerald-500 border-2 border-black rounded-full shadow-lg" title="ออนไลน์"></span>
                    </div>

                    <!-- Overall Rating Summary -->
                    <div class="flex items-center bg-zinc-900/90 border border-zinc-700/80 rounded-2xl px-4 py-2.5 self-start sm:self-end shadow-xl white-glow">
                        <div class="text-white text-2xl font-extrabold mr-3 flex items-center">
                            <i class="fa-solid fa-star text-amber-400 mr-1.5 text-xl"></i>
                            <span id="avgRating">0.0</span>
                        </div>
                        <div class="text-xs text-zinc-400 border-l border-zinc-700 pl-3">
                            <p class="font-medium text-zinc-200">คะแนนรีวิวร้านค้า</p>
                            <p class="text-zinc-500"><span id="totalReviewsCount" class="text-white font-bold">0</span> รีวิวทั้งหมด</p>
                        </div>
                    </div>
                </div>

                <!-- Store Name and Description -->
                <div>
                    <h1 id="storeName" class="text-2xl sm:text-3xl font-extrabold text-white tracking-wide mb-1 flex items-center gap-2">
                        <span>DEFY STORE</span>
                        <i class="fa-solid fa-circle-check text-white text-base" title="ร้านค้าได้รับการยืนยันแล้ว"></i>
                    </h1>
                    <p id="storeDesc" class="text-zinc-400 text-sm sm:text-base leading-relaxed font-light max-w-2xl">
                        ยินดีต้อนรับสู่ร้านค้าของเรา บริการด้วยความจริงใจ จำหน่ายไอเทมเกม สินค้าดิจิทัล และบริการเติมเกมตลอด 24 ชั่วโมง
                    </p>
                </div>
            </div>
        </div>

        <!-- Contact & Payment Section (Grid Layout) -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            
            <!-- Contact Channels Card -->
            <div class="glass-card rounded-3xl p-6 shadow-xl flex flex-col justify-between black-aura">
                <div>
                    <div class="flex items-center space-x-2.5 mb-5 border-b border-zinc-800 pb-3">
                        <div class="w-8 h-8 rounded-lg bg-zinc-800 text-white flex items-center justify-center border border-zinc-700">
                            <i class="fa-solid fa-address-book text-sm"></i>
                        </div>
                        <h2 class="text-base font-bold text-white tracking-wide">ช่องทางการติดต่อ</h2>
                    </div>

                    <!-- Contact Items List -->
                    <div class="space-y-3.5">
                        <!-- LINE Contact -->
                        <div class="flex items-center justify-between p-3.5 rounded-2xl bg-zinc-900/90 border border-zinc-800 hover:border-zinc-700 transition duration-200">
                            <div class="flex items-center space-x-3">
                                <div class="w-10 h-10 rounded-xl bg-[#00B900]/20 border border-[#00B900]/40 text-[#00B900] flex items-center justify-center text-xl shadow-inner">
                                    <i class="fa-brands fa-line"></i>
                                </div>
                                <div>
                                    <span class="text-[10px] font-bold text-zinc-500 uppercase tracking-wider block">Line Official</span>
                                    <span id="lineIdText" class="font-bold text-white text-sm">wjfigxddx</span>
                                </div>
                            </div>
                            <button onclick="copyToClipboard('lineIdText', 'Line ID')" class="px-3.5 py-1.5 bg-white hover:bg-zinc-200 text-black text-xs font-bold rounded-xl transition duration-200 flex items-center space-x-1.5 shadow-md">
                                <i class="fa-regular fa-copy"></i>
                                <span>คัดลอก</span>
                            </button>
                        </div>

                        <!-- Facebook Contact -->
                        <div class="flex items-center justify-between p-3.5 rounded-2xl bg-zinc-900/90 border border-zinc-800 hover:border-zinc-700 transition duration-200">
                            <div class="flex items-center space-x-3">
                                <div class="w-10 h-10 rounded-xl bg-blue-600/20 border border-blue-500/40 text-blue-400 flex items-center justify-center text-xl shadow-inner">
                                    <i class="fa-brands fa-facebook-f"></i>
                                </div>
                                <div>
                                    <span class="text-[10px] font-bold text-zinc-500 uppercase tracking-wider block">Facebook Page</span>
                                    <span id="fbText" class="font-bold text-white text-sm">FB : defy</span>
                                </div>
                            </div>
                            <button onclick="copyToClipboard('fbText', 'Facebook')" class="px-3.5 py-1.5 bg-white hover:bg-zinc-200 text-black text-xs font-bold rounded-xl transition duration-200 flex items-center space-x-1.5 shadow-md">
                                <i class="fa-regular fa-copy"></i>
                                <span>คัดลอก</span>
                            </button>
                        </div>
                    </div>
                </div>

                <p class="text-[11px] text-zinc-500 text-center mt-5">กดปุ่มคัดลอกเพื่อนำไอดีไปค้นหาได้ทันที</p>
            </div>

            <!-- Account Banner Card (16:9 Aspect Ratio 1024x576) -->
            <div class="glass-card rounded-3xl p-6 shadow-xl black-aura flex flex-col justify-between">
                <div>
                    <div class="flex items-center justify-between mb-4 border-b border-zinc-800 pb-3">
                        <div class="flex items-center space-x-2.5">
                            <div class="w-8 h-8 rounded-lg bg-zinc-800 text-white flex items-center justify-center border border-zinc-700">
                                <i class="fa-solid fa-qrcode text-sm"></i>
                            </div>
                            <h2 class="text-base font-bold text-white tracking-wide">ช่องทางชำระเงิน / บัญชี</h2>
                        </div>
                        <span class="text-[10px] bg-zinc-800 text-zinc-400 border border-zinc-700 font-medium px-2 py-0.5 rounded-full">16:9 (1024x576)</span>
                    </div>

                    <!-- 16:9 Horizontal Account Card Frame -->
                    <div class="relative rounded-2xl overflow-hidden border border-zinc-800 bg-black group aspect-banner shadow-2xl">
                        <img id="accountBannerImg" 
                             src="https://placehold.co/1024x576/09090b/ffffff?text=16:9+Payment+Card+(1024x576)" 
                             alt="ป้ายบัญชี 16:9" 
                             class="w-full h-full object-cover filter contrast-105">
                        
                        <div class="absolute inset-0 bg-black/60 opacity-0 group-hover:opacity-100 transition duration-300 flex items-center justify-center backdrop-blur-sm">
                            <button onclick="openImageModal(storeData.accountBannerUrl)" class="px-4 py-2 bg-white text-black hover:bg-zinc-200 rounded-xl text-xs font-bold shadow-xl transition flex items-center space-x-1">
                                <i class="fa-solid fa-expand mr-1"></i> ดูภาพขนาดเต็ม
                            </button>
                        </div>
                    </div>

                    <!-- Quick Transfer Line Info Notice Box with Copy Button -->
                    <div class="mt-4 p-3.5 bg-zinc-900/90 border border-zinc-800 rounded-2xl flex flex-col sm:flex-row sm:items-center justify-between gap-3 shadow-lg">
                        <div class="flex items-center space-x-2.5">
                            <div class="w-8 h-8 rounded-xl bg-emerald-500/20 text-emerald-400 border border-emerald-500/40 flex items-center justify-center text-sm shrink-0">
                                <i class="fa-solid fa-money-bill-transfer"></i>
                            </div>
                            <div>
                                <span class="text-[10px] font-bold text-zinc-400 block uppercase tracking-wider">คัดลอกข้อความเพื่อโอนเงิน</span>
                                <span id="fullTransferMessage" class="text-xs font-bold text-white">โอนเงินให้ line <strong id="transferLineText" class="text-emerald-400 underline font-extrabold">wjfigxddx</strong></span>
                            </div>
                        </div>
                        <button onclick="copyToClipboard('fullTransferMessage', 'ข้อความโอนเงิน')" class="px-3.5 py-2 bg-white hover:bg-zinc-200 text-black text-xs font-bold rounded-xl transition duration-200 flex items-center justify-center space-x-1.5 shadow-md shrink-0">
                            <i class="fa-regular fa-copy"></i>
                            <span>คัดลอกเพื่อโอนเงิน</span>
                        </button>
                    </div>
                </div>
                <p class="text-[11px] text-zinc-500 text-center mt-3">สามารถคลิกดูภาพป้ายบัญชีฉบับเต็มได้</p>
            </div>

        </div>

        <!-- Reviews & Comments Section -->
        <div class="glass-card rounded-3xl p-6 shadow-2xl mb-12 black-aura">
            <div class="flex items-center justify-between pb-4 mb-6 border-b border-zinc-800">
                <div class="flex items-center space-x-2.5">
                    <div class="w-8 h-8 rounded-lg bg-white text-black flex items-center justify-center font-bold">
                        <i class="fa-solid fa-comments text-sm"></i>
                    </div>
                    <h2 class="text-xl font-bold text-white tracking-wide">รีวิวและความคิดเห็นจากลูกค้า</h2>
                </div>
            </div>

            <!-- Enhanced Customer Review Form -->
            <div class="bg-zinc-950/90 rounded-2xl p-5 border border-zinc-800 mb-8 shadow-inner">
                <h3 class="font-bold text-white text-base mb-4 flex items-center">
                    <i class="fa-solid fa-pen-to-square text-zinc-400 mr-2"></i> เขียนรีวิวของคุณ (สำหรับลูกค้า)
                </h3>
                
                <form id="reviewForm" onsubmit="handleReviewSubmit(event)" class="space-y-4">
                    <!-- Rating Stars Selector -->
                    <div>
                        <label class="block text-xs font-bold text-zinc-400 mb-1.5">ให้คะแนนความประทับใจ (1 - 5 ดาว) <span class="text-rose-400">*</span></label>
                        <div class="flex items-center space-x-3" id="starRatingContainer">
                            <div class="flex text-2xl text-amber-400 cursor-pointer space-x-1" id="ratingStars">
                                <i class="fa-solid fa-star star-btn transition hover:scale-110" data-value="1"></i>
                                <i class="fa-solid fa-star star-btn transition hover:scale-110" data-value="2"></i>
                                <i class="fa-solid fa-star star-btn transition hover:scale-110" data-value="3"></i>
                                <i class="fa-solid fa-star star-btn transition hover:scale-110" data-value="4"></i>
                                <i class="fa-solid fa-star star-btn transition hover:scale-110" data-value="5"></i>
                            </div>
                            <span id="ratingScoreText" class="text-xs font-bold bg-zinc-800 border border-zinc-700 text-amber-300 px-2.5 py-1 rounded-lg">5.0 คะแนน</span>
                        </div>
                    </div>

                    <!-- Customer Info Input & Avatar Customization -->
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <!-- User Name -->
                        <div>
                            <label class="block text-xs font-bold text-zinc-400 mb-1.5">ชื่อผู้รีวิว / นามแฝง <span class="text-rose-400">*</span></label>
                            <input type="text" id="reviewerName" required placeholder="เช่น น้องมีน, ลูกค้าประจำ" 
                                   class="w-full px-3.5 py-2.5 rounded-xl bg-zinc-900 border border-zinc-700 focus:outline-none focus:border-white text-sm text-white placeholder-zinc-600">
                        </div>

                        <!-- Customer Profile Image Customization (URL or Direct Upload) -->
                        <div>
                            <label class="block text-xs font-bold text-zinc-400 mb-1.5">ตั้งค่ารูปโปรไฟล์ส่วนตัว (อัปโหลด หรือ URL)</label>
                            <div class="flex gap-2">
                                <input type="text" id="reviewerAvatar" placeholder="URL รูปโปรไฟล์ หรือ อัปโหลด" 
                                       class="flex-1 px-3.5 py-2.5 rounded-xl bg-zinc-900 border border-zinc-700 focus:outline-none focus:border-white text-sm text-white placeholder-zinc-600">
                                <label class="cursor-pointer bg-zinc-800 hover:bg-zinc-700 border border-zinc-700 text-white text-xs font-bold px-3 py-2.5 rounded-xl flex items-center justify-center transition shrink-0">
                                    <i class="fa-solid fa-upload mr-1 text-zinc-400"></i> เลือกรูป
                                    <input type="file" accept="image/*" class="hidden" onchange="handleFileUpload(event, 'reviewerAvatar', 'avatarPreview')">
                                </label>
                            </div>
                        </div>
                    </div>

                    <!-- Customer Profile Preview Indicator -->
                    <div id="avatarPreviewContainer" class="hidden items-center space-x-2 pt-1">
                        <span class="text-xs text-zinc-400">พรีวิวรูปโปรไฟล์ที่คุณเลือก:</span>
                        <img id="avatarPreview" src="" class="w-7 h-7 rounded-full border border-zinc-600 object-cover">
                    </div>

                    <!-- Comment Textarea -->
                    <div>
                        <label class="block text-xs font-bold text-zinc-400 mb-1.5">ข้อความรีวิวความคิดเห็น <span class="text-rose-400">*</span></label>
                        <textarea id="reviewerComment" rows="3" required placeholder="บอกเล่าประสบการณ์ ความประทับใจในบริการ..." 
                                  class="w-full px-3.5 py-2.5 rounded-xl bg-zinc-900 border border-zinc-700 focus:outline-none focus:border-white text-sm text-white placeholder-zinc-600 leading-relaxed"></textarea>
                    </div>

                    <!-- Transaction / Chat Screenshot Attachment Feature -->
                    <div class="bg-zinc-900/60 p-3.5 rounded-xl border border-zinc-800">
                        <label class="block text-xs font-bold text-zinc-300 mb-1.5">
                            <i class="fa-solid fa-paperclip text-zinc-400 mr-1"></i> แนบภาพหลักฐาน / ภาพแชทการซื้อขาย (ถ้ามี)
                        </label>
                        <div class="flex flex-col sm:flex-row gap-2">
                            <input type="text" id="reviewerProof" placeholder="ระบุ URL รูปแชทการซื้อขาย" 
                                   class="flex-1 px-3.5 py-2 rounded-xl bg-zinc-950 border border-zinc-800 focus:outline-none focus:border-white text-xs text-white placeholder-zinc-600">
                            <label class="cursor-pointer bg-zinc-800 hover:bg-zinc-700 border border-zinc-700 text-zinc-200 text-xs font-bold px-4 py-2 rounded-xl flex items-center justify-center transition shrink-0">
                                <i class="fa-solid fa-image mr-1.5 text-zinc-400"></i> แนบรูปแชท
                                <input type="file" accept="image/*" class="hidden" onchange="handleFileUpload(event, 'reviewerProof', 'proofPreview')">
                            </label>
                        </div>
                        
                        <!-- Proof Image Thumbnail Preview -->
                        <div id="proofPreviewContainer" class="hidden mt-3 relative inline-block">
                            <p class="text-[11px] text-zinc-400 mb-1">ตัวอย่างภาพแชทที่จะแนบ:</p>
                            <div class="relative rounded-lg overflow-hidden border border-zinc-700 w-32 h-20 bg-black">
                                <img id="proofPreview" src="" class="w-full h-full object-cover">
                                <button type="button" onclick="removeAttachedProof()" class="absolute top-1 right-1 bg-black/80 hover:bg-rose-600 text-white rounded-full w-5 h-5 flex items-center justify-center text-[10px]">
                                    <i class="fa-solid fa-xmark"></i>
                                </button>
                            </div>
                        </div>
                    </div>

                    <button type="submit" class="w-full sm:w-auto px-7 py-3 bg-white hover:bg-zinc-200 text-black font-extrabold text-sm rounded-xl shadow-lg transition duration-200 flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-paper-plane"></i>
                        <span>โพสต์รีวิวความคิดเห็น</span>
                    </button>
                </form>
            </div>

            <!-- Comment List Header -->
            <div class="flex justify-between items-center mb-5">
                <h3 class="font-bold text-zinc-300 text-sm tracking-wide">ความคิดเห็นทั้งหมด</h3>
                <span id="adminNotice" class="hidden text-xs bg-rose-950/80 border border-rose-800 text-rose-300 font-bold px-3 py-1 rounded-full">
                    <i class="fa-solid fa-shield-halved mr-1"></i> โหมดหลังบ้าน: คุณสามารถเลือกสิทธิ์ลบคอมเมนต์ได้
                </span>
            </div>

            <!-- Comments Container -->
            <div id="commentsList" class="space-y-4">
                <!-- Dynamic Comment items injected via JS -->
            </div>
        </div>
    </main>

    <!-- Admin Login Modal -->
    <div id="adminAuthModal" class="fixed inset-0 bg-black/80 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-zinc-900 rounded-3xl max-w-sm w-full p-6 shadow-2xl border border-zinc-800">
            <div class="text-center mb-5">
                <div class="w-12 h-12 bg-white text-black rounded-2xl flex items-center justify-center mx-auto mb-3 text-xl font-bold shadow-lg">
                    <i class="fa-solid fa-lock"></i>
                </div>
                <h3 class="text-lg font-bold text-white">ยืนยันสิทธิ์หลังบ้าน (Admin)</h3>
                <p class="text-xs text-zinc-400 mt-1">กรอกรหัสผ่านเพื่อเข้าสู่ระบบปรับแต่งร้านค้า</p>
            </div>
            
            <form onsubmit="handleAdminAuth(event)">
                <div class="mb-5">
                    <input type="password" id="adminPassword" placeholder="รหัสผ่าน (เริ่มต้น: 1234)" required
                           class="w-full px-4 py-2.5 bg-zinc-950 border border-zinc-700 rounded-xl text-center text-white font-bold tracking-widest focus:ring-2 focus:ring-white focus:outline-none">
                    <p class="text-[11px] text-zinc-500 text-center mt-2">รหัสทดสอบกดเข้าได้ทันที: <b class="text-zinc-300">1234</b></p>
                </div>
                <div class="flex space-x-2">
                    <button type="button" onclick="closeAdminAuthModal()" class="w-1/2 py-2.5 bg-zinc-800 text-zinc-300 font-bold rounded-xl text-sm hover:bg-zinc-700">ยกเลิก</button>
                    <button type="submit" class="w-1/2 py-2.5 bg-white text-black font-bold rounded-xl text-sm hover:bg-zinc-200">เข้าสู่ระบบ</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Admin Settings Modal -->
    <div id="adminPanelModal" class="fixed inset-0 bg-black/85 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-zinc-900 rounded-3xl max-w-2xl w-full max-h-[90vh] overflow-hidden flex flex-col shadow-2xl border border-zinc-800">
            <!-- Modal Header -->
            <div class="px-6 py-4 bg-zinc-950 border-b border-zinc-800 text-white flex justify-between items-center">
                <div class="flex items-center space-x-2.5">
                    <i class="fa-solid fa-sliders text-zinc-300"></i>
                    <h3 class="font-bold text-base tracking-wide">ปรับแต่งข้อมูลร้านค้า (หลังบ้าน)</h3>
                </div>
                <button onclick="closeAdminPanel()" class="text-zinc-400 hover:text-white text-lg">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Modal Form Body -->
            <div class="p-6 overflow-y-auto space-y-5">
                <!-- Store Info -->
                <div class="space-y-3">
                    <h4 class="text-xs font-bold uppercase tracking-wider text-zinc-400">ข้อมูลหลักของร้าน</h4>
                    <div>
                        <label class="block text-xs font-bold text-zinc-300 mb-1">ชื่อร้านค้า</label>
                        <input type="text" id="editStoreName" class="w-full px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white focus:outline-none focus:border-white">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-zinc-300 mb-1">คำอธิบายร้านค้า</label>
                        <textarea id="editStoreDesc" rows="2" class="w-full px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white focus:outline-none focus:border-white"></textarea>
                    </div>
                </div>

                <hr class="border-zinc-800">

                <!-- Images Settings -->
                <div class="space-y-4">
                    <h4 class="text-xs font-bold uppercase tracking-wider text-zinc-400">รูปภาพประจำร้าน & ปก</h4>
                    
                    <!-- Profile Image -->
                    <div>
                        <label class="block text-xs font-bold text-zinc-300 mb-1">รูปโปรไฟล์ร้านค้า (Profile)</label>
                        <div class="flex gap-2">
                            <input type="text" id="editProfileUrl" placeholder="ระบุ URL รูปภาพ" class="flex-1 px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white">
                            <label class="cursor-pointer bg-zinc-800 hover:bg-zinc-700 border border-zinc-700 text-zinc-200 text-xs font-bold px-3.5 py-2 rounded-xl flex items-center">
                                <i class="fa-solid fa-upload mr-1 text-zinc-400"></i> เลือกรูป
                                <input type="file" accept="image/*" class="hidden" onchange="handleFileUpload(event, 'editProfileUrl')">
                            </label>
                        </div>
                    </div>

                    <!-- Cover Image -->
                    <div>
                        <label class="block text-xs font-bold text-zinc-300 mb-1">รูปปกหลังร้านค้า (Cover Photo)</label>
                        <div class="flex gap-2">
                            <input type="text" id="editCoverUrl" placeholder="ระบุ URL รูปภาพ" class="flex-1 px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white">
                            <label class="cursor-pointer bg-zinc-800 hover:bg-zinc-700 border border-zinc-700 text-zinc-200 text-xs font-bold px-3.5 py-2 rounded-xl flex items-center">
                                <i class="fa-solid fa-upload mr-1 text-zinc-400"></i> เลือกรูป
                                <input type="file" accept="image/*" class="hidden" onchange="handleFileUpload(event, 'editCoverUrl')">
                            </label>
                        </div>
                    </div>

                    <!-- 16:9 Account Banner -->
                    <div class="bg-zinc-950 p-3.5 rounded-2xl border border-zinc-800">
                        <label class="block text-xs font-bold text-white mb-1">
                            <i class="fa-regular fa-image text-zinc-400 mr-1"></i> ป้ายบัญชี 16:9 (ขนาดแนะนำ 1024 × 576 พิกเซล)
                        </label>
                        <div class="flex gap-2 mb-1.5">
                            <input type="text" id="editAccountBannerUrl" placeholder="ระบุ URL รูปภาพ" class="flex-1 px-3.5 py-2 bg-zinc-900 border border-zinc-700 rounded-xl text-sm text-white">
                            <label class="cursor-pointer bg-white hover:bg-zinc-200 text-black text-xs font-bold px-3.5 py-2 rounded-xl flex items-center shadow-md">
                                <i class="fa-solid fa-upload mr-1"></i> อัปโหลด
                                <input type="file" accept="image/*" class="hidden" onchange="handleFileUpload(event, 'editAccountBannerUrl')">
                            </label>
                        </div>
                        <p class="text-[11px] text-zinc-500">ภาพจะปรับแสดงผลแบบอัตราส่วนแนวนอน 16:9 พอดีกรอบ</p>
                    </div>
                </div>

                <hr class="border-zinc-800">

                <!-- Contact Channel Settings -->
                <div class="space-y-3">
                    <h4 class="text-xs font-bold uppercase tracking-wider text-zinc-400">ช่องทางการติดต่อ</h4>
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-bold text-zinc-300 mb-1">Line ID</label>
                            <input type="text" id="editLineId" class="w-full px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white">
                        </div>
                        <div>
                            <label class="block text-xs font-bold text-zinc-300 mb-1">Facebook</label>
                            <input type="text" id="editFb" class="w-full px-3.5 py-2 bg-zinc-950 border border-zinc-700 rounded-xl text-sm text-white">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Modal Footer -->
            <div class="px-6 py-4 bg-zinc-950 border-t border-zinc-800 flex justify-between items-center">
                <button onclick="resetDataToDefault()" class="text-xs text-rose-400 font-bold hover:underline flex items-center gap-1">
                    <i class="fa-solid fa-rotate-left"></i> รีเซ็ตเป็นค่าเริ่มต้น
                </button>
                <div class="flex space-x-2">
                    <button onclick="logoutAdmin()" id="adminLogoutBtn" class="px-4 py-2 bg-rose-950/80 hover:bg-rose-900 text-rose-300 border border-rose-800 font-bold rounded-xl text-sm transition">
                        <i class="fa-solid fa-right-from-bracket mr-1"></i> ออกจากระบบ
                    </button>
                    <button onclick="closeAdminPanel()" class="px-4 py-2 bg-zinc-800 text-zinc-300 font-bold rounded-xl text-sm hover:bg-zinc-700">ยกเลิก</button>
                    <button onclick="saveAdminSettings()" class="px-5 py-2 bg-white text-black font-bold rounded-xl text-sm hover:bg-zinc-200 shadow-md">
                        <i class="fa-solid fa-floppy-disk mr-1"></i> บันทึกข้อมูล
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Image Fullscreen Lightbox Modal -->
    <div id="imageModal" class="fixed inset-0 bg-black/95 backdrop-blur-lg z-50 flex items-center justify-center p-4 hidden" onclick="closeImageModal()">
        <div class="relative max-w-4xl w-full bg-zinc-950 rounded-3xl border border-zinc-800 overflow-hidden p-3 shadow-2xl" onclick="event.stopPropagation()">
            <button onclick="closeImageModal()" class="absolute top-4 right-4 bg-zinc-800/80 hover:bg-white hover:text-black text-white w-9 h-9 rounded-full flex items-center justify-center transition z-20">
                <i class="fa-solid fa-xmark"></i>
            </button>
            <div class="flex justify-center items-center overflow-hidden max-h-[82vh]">
                <img id="modalPreviewImg" src="" alt="ภาพขยาย" class="max-w-full max-h-[80vh] object-contain rounded-2xl shadow-2xl">
            </div>
            <p id="modalCaption" class="text-center text-xs text-zinc-400 py-2.5 font-light">ดูภาพขยายขนาดเต็ม</p>
        </div>
    </div>

    <!-- Direct Owner Live Chat Widget -->
    <div class="fixed bottom-5 right-5 z-40">
        <!-- Floating Toggle Button -->
        <button id="chatToggleBtn" onclick="toggleDirectChat()" class="relative bg-white text-black p-4 rounded-full shadow-2xl hover:scale-105 active:scale-95 transition-all duration-300 flex items-center justify-center group ring-4 ring-black/50">
            <i class="fa-solid fa-comments text-2xl group-hover:rotate-12 transition transform"></i>
            <span class="absolute -top-1 -right-1 w-4 h-4 bg-emerald-500 rounded-full border-2 border-black"></span>
            <span class="max-w-0 overflow-hidden whitespace-nowrap group-hover:max-w-xs transition-all duration-500 ease-in-out font-bold text-xs pl-0 group-hover:pl-2">
                สอบถามเจ้าของเว็บ
            </span>
        </button>

        <!-- Live Chat Floating Window -->
        <div id="directChatBox" class="hidden absolute bottom-16 right-0 w-[88vw] sm:w-80 bg-zinc-950 border border-zinc-800 rounded-3xl shadow-2xl overflow-hidden flex flex-col black-aura transition-all transform duration-300">
            <!-- Chat Window Header -->
            <div class="bg-zinc-900 px-4 py-3 border-b border-zinc-800 flex items-center justify-between">
                <div class="flex items-center space-x-2.5">
                    <div class="relative">
                        <img id="chatOwnerAvatar" src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=300&auto=format&fit=crop" class="w-8 h-8 rounded-full object-cover border border-zinc-700">
                        <span class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-emerald-500 rounded-full border border-black"></span>
                    </div>
                    <div>
                        <h4 class="text-xs font-bold text-white leading-tight flex items-center gap-1">
                            <span id="chatOwnerName">DEFY STORE</span>
                            <i class="fa-solid fa-circle-check text-emerald-400 text-[10px]"></i>
                        </h4>
                        <span class="text-[10px] text-emerald-400 font-medium">เจ้าของร้านออนไลน์</span>
                    </div>
                </div>
                <button onclick="toggleDirectChat()" class="text-zinc-400 hover:text-white text-sm w-7 h-7 rounded-full flex items-center justify-center hover:bg-zinc-800">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <!-- Chat Messages Log Container -->
            <div id="chatMessages" class="p-4 h-72 overflow-y-auto space-y-3 bg-zinc-950/90 text-xs">
                <!-- Welcome Message from Owner -->
                <div class="flex items-start space-x-2">
                    <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=300&auto=format&fit=crop" class="w-6 h-6 rounded-full object-cover border border-zinc-700 mt-1 shrink-0">
                    <div class="bg-zinc-900 border border-zinc-800 text-zinc-200 p-2.5 rounded-2xl rounded-tl-none max-w-[80%] leading-relaxed shadow-sm">
                        สวัสดีครับ ยินดีต้อนรับสู่ DEFY STORE สอบถามสินค้าหรือต้องการแจ้งโอนเงินสามารถทักแชทสอบถามตรงนี้ได้เลยครับ!
                    </div>
                </div>
            </div>

            <!-- Chat Input Area -->
            <form onsubmit="handleSendDirectChat(event)" class="p-2.5 bg-zinc-900 border-t border-zinc-800 flex items-center space-x-2">
                <input type="text" id="chatInputText" placeholder="พิมพ์ข้อความสอบถาม..." required
                       class="flex-1 bg-zinc-950 border border-zinc-800 text-xs text-white px-3 py-2 rounded-xl focus:outline-none focus:border-white placeholder-zinc-500">
                <button type="submit" class="w-8 h-8 bg-white hover:bg-zinc-200 text-black rounded-xl flex items-center justify-center shrink-0 shadow-md">
                    <i class="fa-solid fa-paper-plane text-xs"></i>
                </button>
            </form>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-6 right-6 bg-zinc-900 border border-zinc-700 text-white text-xs font-bold px-4 py-3 rounded-2xl shadow-2xl z-50 flex items-center space-x-2.5 transition-all transform translate-y-24 opacity-0">
        <i class="fa-solid fa-circle-check text-white text-base" id="toastIcon"></i>
        <span id="toastMessage">แจ้งเตือนสำเร็จ</span>
    </div>

    <script>
        const DEFAULT_STORE_DATA = {
            name: "DEFY STORE",
            desc: "ยินดีต้อนรับสู่ร้านค้าของเรา บริการด้วยความจริงใจ พร็อพเกม สินค้าดิจิทัล และบริการเติมเกมตลอด 24 ชั่วโมง",
            coverUrl: "https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=1200&auto=format&fit=crop",
            profileUrl: "https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=300&auto=format&fit=crop",
            lineId: "wjfigxddx",
            fb: "FB : defy",
            accountBannerUrl: "https://placehold.co/1024x576/09090b/ffffff?text=DEFY+STORE+16:9+PAYMENT+(1024x576)"
        };

        const DEFAULT_REVIEWS = [
            {
                id: 1,
                name: "ลูกค้าประจำ (กอล์ฟ)",
                rating: 5,
                comment: "สั่งเติมกับทางร้านไวมาก คุยง่าย พ่อค้าดูแลดีมากครับ มีแนบสลิป/หลักฐานตรงแชทให้เรียบร้อย ประทับใจ 10/10",
                date: "12 ก.ย. 2026",
                avatar: "https://api.dicebear.com/7.x/bottts/svg?seed=Customer1",
                proofUrl: "https://placehold.co/800x450/18181b/ffffff?text=Chat+Trade+Proof+Screenshot",
                reply: "ขอบคุณมากครับยินดีให้บริการเสมอครับผม!"
            },
            {
                id: 2,
                name: "มินนี่ ช็อป",
                rating: 5,
                comment: "บริการประทับใจมากค่ะ ป้ายชำระเงินแนวนอนชัดเจน ซื้อขายรวดเร็วทันใจค่ะ",
                date: "10 ก.ย. 2026",
                avatar: "https://api.dicebear.com/7.x/bottts/svg?seed=Minnie",
                proofUrl: "",
                reply: ""
            }
        ];

        // State Variables
        let storeData = loadStoreData();
        let reviews = loadReviewsData();
        let isAdmin = sessionStorage.getItem('defy_admin_session') === 'true';
        let currentRatingInput = 5;

        function loadStoreData() {
            try {
                const data = localStorage.getItem('defy_store_data_v2');
                return data ? JSON.parse(data) : { ...DEFAULT_STORE_DATA };
            } catch (e) {
                console.error("Error loading store data:", e);
                return { ...DEFAULT_STORE_DATA };
            }
        }

        function loadReviewsData() {
            try {
                const data = localStorage.getItem('defy_store_reviews_v2');
                return data ? JSON.parse(data) : [ ...DEFAULT_REVIEWS ];
            } catch (e) {
                console.error("Error loading reviews:", e);
                return [ ...DEFAULT_REVIEWS ];
            }
        }

        function initAnimatedBgCanvas() {
            const canvas = document.getElementById('bgCanvas');
            if (!canvas) return;
            const ctx = canvas.getContext('2d');

            let width = canvas.width = window.innerWidth;
            let height = canvas.height = window.innerHeight;

            window.addEventListener('resize', () => {
                width = canvas.width = window.innerWidth;
                height = canvas.height = window.innerHeight;
            });

            // Continuous Falling Snowflakes
            const numFlakes = 45;
            const flakes = [];
            for (let i = 0; i < numFlakes; i++) {
                flakes.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    radius: Math.random() * 2 + 0.8,
                    speedY: Math.random() * 0.7 + 0.3,
                    speedX: (Math.random() - 0.5) * 0.4,
                    opacity: Math.random() * 0.5 + 0.2
                });
            }

            // Continuous Motion Black/White Fog & Smoke Particles
            const numSmokePuffs = 12;
            const smokePuffs = [];
            for (let i = 0; i < numSmokePuffs; i++) {
                smokePuffs.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    radius: Math.random() * 150 + 100,
                    dx: (Math.random() - 0.5) * 0.3,
                    dy: (Math.random() - 0.5) * 0.2 - 0.1,
                    alpha: Math.random() * 0.08 + 0.02,
                    isWhite: Math.random() > 0.4
                });
            }

            function animateBackground() {
                ctx.clearRect(0, 0, width, height);

                for (let i = 0; i < numSmokePuffs; i++) {
                    const p = smokePuffs[i];
                    p.x += p.dx;
                    p.y += p.dy;

                    if (p.x < -p.radius) p.x = width + p.radius;
                    if (p.x > width + p.radius) p.x = -p.radius;
                    if (p.y < -p.radius) p.y = height + p.radius;
                    if (p.y > height + p.radius) p.y = -p.radius;

                    const grad = ctx.createRadialGradient(p.x, p.y, 0, p.x, p.y, p.radius);
                    if (p.isWhite) {
                        grad.addColorStop(0, `rgba(255, 255, 255, ${p.alpha})`);
                        grad.addColorStop(0.5, `rgba(180, 180, 190, ${p.alpha * 0.4})`);
                        grad.addColorStop(1, 'rgba(0, 0, 0, 0)');
                    } else {
                        grad.addColorStop(0, `rgba(20, 20, 25, ${p.alpha * 1.5})`);
                        grad.addColorStop(0.6, `rgba(10, 10, 12, ${p.alpha * 0.8})`);
                        grad.addColorStop(1, 'rgba(0, 0, 0, 0)');
                    }

                    ctx.fillStyle = grad;
                    ctx.beginPath();
                    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
                    ctx.fill();
                }

                for (let i = 0; i < numFlakes; i++) {
                    const f = flakes[i];
                    ctx.beginPath();
                    ctx.arc(f.x, f.y, f.radius, 0, Math.PI * 2);
                    ctx.fillStyle = `rgba(255, 255, 255, ${f.opacity})`;
                    ctx.fill();

                    f.y += f.speedY;
                    f.x += f.speedX;

                    if (f.y > height) {
                        f.y = -5;
                        f.x = Math.random() * width;
                    }
                    if (f.x > width) f.x = 0;
                    if (f.x < 0) f.x = width;
                }

                requestAnimationFrame(animateBackground);
            }

            animateBackground();
        }

        window.addEventListener('DOMContentLoaded', () => {
            initAnimatedBgCanvas();
            renderStoreData();
            renderReviews();
            setupStarRatingEvents();
            checkAdminStatusUI();

            // Keyboard shortcut for closing modals
            window.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    closeAdminAuthModal();
                    closeAdminPanel();
                    closeImageModal();
                }
            });
        });

        function renderStoreData() {
            document.getElementById('navStoreName').textContent = storeData.name || "DEFY STORE";
            const nameEl = document.getElementById('storeName');
            if (nameEl && nameEl.children[0]) {
                nameEl.children[0].textContent = storeData.name || "DEFY STORE";
            }
            document.getElementById('storeDesc').textContent = storeData.desc || "";
            document.getElementById('coverImg').src = storeData.coverUrl || DEFAULT_STORE_DATA.coverUrl;
            document.getElementById('profileImg').src = storeData.profileUrl || DEFAULT_STORE_DATA.profileUrl;
            document.getElementById('lineIdText').textContent = storeData.lineId || "";
            if (document.getElementById('transferLineText')) {
                document.getElementById('transferLineText').textContent = storeData.lineId || "";
            }
            document.getElementById('fbText').textContent = storeData.fb || "";
            document.getElementById('accountBannerImg').src = storeData.accountBannerUrl || DEFAULT_STORE_DATA.accountBannerUrl;
            
            if (document.getElementById('chatOwnerAvatar')) {
                document.getElementById('chatOwnerAvatar').src = storeData.profileUrl || DEFAULT_STORE_DATA.profileUrl;
            }
            if (document.getElementById('chatOwnerName')) {
                document.getElementById('chatOwnerName').textContent = storeData.name || "DEFY STORE";
            }
        }

        function toggleDirectChat() {
            const chatBox = document.getElementById('directChatBox');
            chatBox.classList.toggle('hidden');
        }

        function handleSendDirectChat(e) {
            e.preventDefault();
            const input = document.getElementById('chatInputText');
            const message = input.value.trim();
            if (!message) return;

            const chatLog = document.getElementById('chatMessages');

            const userMsgEl = document.createElement('div');
            userMsgEl.className = "flex items-start justify-end space-x-2";
            userMsgEl.innerHTML = `
                <div class="bg-white text-black p-2.5 rounded-2xl rounded-tr-none max-w-[80%] leading-relaxed shadow-sm font-medium">
                    ${escapeHtml(message)}
                </div>
            `;
            chatLog.appendChild(userMsgEl);
            input.value = '';

            chatLog.scrollTop = chatLog.scrollHeight;

            setTimeout(() => {
                const autoReplyEl = document.createElement('div');
                autoReplyEl.className = "flex items-start space-x-2";
                autoReplyEl.innerHTML = `
                    <img src="${escapeHtml(storeData.profileUrl)}" class="w-6 h-6 rounded-full object-cover border border-zinc-700 mt-1 shrink-0">
                    <div class="bg-zinc-900 border border-zinc-800 text-zinc-200 p-2.5 rounded-2xl rounded-tl-none max-w-[80%] leading-relaxed shadow-sm">
                        รับทราบครับ! เจ้าของร้านได้รับข้อความของคุณแล้ว จะรีบตอบกลับผ่านช่องทางนี้ หรือติดต่อทาง Line ID: <strong>${escapeHtml(storeData.lineId)}</strong> ได้เลยครับ
                    </div>
                `;
                chatLog.appendChild(autoReplyEl);
                chatLog.scrollTop = chatLog.scrollHeight;
            }, 800);
        }

        function renderReviews() {
            const container = document.getElementById('commentsList');
            container.innerHTML = '';

            if (reviews.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-zinc-500 bg-zinc-950/60 rounded-2xl border border-dashed border-zinc-800">
                        <i class="fa-regular fa-comment-dots text-4xl mb-3 text-zinc-600"></i>
                        <p class="text-sm font-medium">ยังไม่มีความคิดเห็น เป็นคนแรกที่ร่วมรีวิวให้คะแนนร้านค้านี้!</p>
                    </div>
                `;
                document.getElementById('avgRating').textContent = "0.0";
                document.getElementById('totalReviewsCount').textContent = "0";
                return;
            }

            const totalScore = reviews.reduce((acc, curr) => acc + (Number(curr.rating) || 0), 0);
            const avg = (totalScore / reviews.length).toFixed(1);

            document.getElementById('avgRating').textContent = avg;
            document.getElementById('totalReviewsCount').textContent = reviews.length;

            reviews.forEach((item) => {
                const starsHtml = Array.from({ length: 5 }, (_, i) => {
                    return `<i class="fa-solid fa-star ${i < item.rating ? 'text-amber-400' : 'text-zinc-700'}"></i>`;
                }).join('');

                const commentEl = document.createElement('div');
                commentEl.className = "bg-zinc-900/90 p-4 sm:p-5 rounded-2xl border border-zinc-800/80 relative group hover:border-zinc-700 transition duration-200 shadow-xl";
                
                commentEl.innerHTML = `
                    <div class="flex items-start justify-between gap-3">
                        <div class="flex items-center space-x-3">
                            <img src="${escapeHtml(item.avatar || 'https://api.dicebear.com/7.x/bottts/svg?seed=' + encodeURIComponent(item.name))}" 
                                 alt="Avatar" class="w-11 h-11 rounded-full border border-zinc-700 object-cover bg-zinc-950 shadow-md">
                            <div>
                                <h4 class="font-bold text-white text-sm flex items-center gap-1.5">
                                    <span>${escapeHtml(item.name)}</span>
                                </h4>
                                <div class="flex items-center space-x-2 mt-0.5">
                                    <div class="text-xs text-amber-400 flex space-x-0.5">${starsHtml}</div>
                                    <span class="text-[11px] text-zinc-500">• ${escapeHtml(item.date)}</span>
                                </div>
                            </div>
                        </div>

                        ${isAdmin ? `
                            <div class="flex items-center space-x-1">
                                <button onclick="promptOwnerReply(${item.id})" class="text-xs text-zinc-300 hover:text-white bg-zinc-800 hover:bg-zinc-700 border border-zinc-700 px-2.5 py-1 rounded-xl font-medium transition">
                                    <i class="fa-solid fa-reply mr-1"></i> ตอบกลับ
                                </button>
                                <button onclick="deleteReview(${item.id})" class="text-xs text-rose-400 hover:text-white bg-rose-950/60 hover:bg-rose-900 border border-rose-800/80 px-2.5 py-1 rounded-xl font-bold transition">
                                    <i class="fa-solid fa-trash-can"></i>
                                </button>
                            </div>
                        ` : ''}
                    </div>

                    <p class="text-zinc-300 text-sm mt-3 leading-relaxed font-light">${escapeHtml(item.comment)}</p>

                    ${item.proofUrl ? `
                        <div class="mt-4 pt-3 border-t border-zinc-800/80">
                            <p class="text-[11px] font-bold text-zinc-400 mb-2 flex items-center">
                                <i class="fa-solid fa-paperclip text-zinc-500 mr-1.5"></i> ภาพแนบหลักฐาน / แชทการซื้อขาย
                            </p>
                            <div class="relative w-44 sm:w-52 aspect-banner rounded-xl overflow-hidden border border-zinc-700 bg-black group/img cursor-pointer" onclick="openImageModal('${escapeHtml(item.proofUrl)}', 'หลักฐานการซื้อขายของ ${escapeHtml(item.name)}')">
                                <img src="${escapeHtml(item.proofUrl)}" alt="หลักฐานแชท" class="w-full h-full object-cover group-hover/img:scale-105 transition duration-300">
                                <div class="absolute inset-0 bg-black/40 opacity-0 group-hover/img:opacity-100 transition flex items-center justify-center">
                                    <span class="text-[10px] bg-white text-black px-2.5 py-1 rounded-full font-bold shadow-md">
                                        <i class="fa-solid fa-expand mr-1"></i> ดูภาพขยาย
                                    </span>
                                </div>
                            </div>
                        </div>
                    ` : ''}

                    ${item.reply ? `
                        <div class="mt-3 p-3 bg-zinc-950 border border-zinc-800/90 rounded-xl flex items-start space-x-2.5">
                            <div class="w-6 h-6 rounded-full bg-white text-black flex items-center justify-center text-[10px] font-bold shrink-0 mt-0.5">
                                <i class="fa-solid fa-store"></i>
                            </div>
                            <div class="flex-1 text-xs">
                                <span class="font-bold text-white block">ตอบกลับจากทางร้าน:</span>
                                <p class="text-zinc-400 mt-0.5 font-light">${escapeHtml(item.reply)}</p>
                            </div>
                        </div>
                    ` : ''}
                `;

                container.appendChild(commentEl);
            });
        }

        function setupStarRatingEvents() {
            const stars = document.querySelectorAll('#ratingStars .star-btn');
            stars.forEach(star => {
                star.addEventListener('click', function() {
                    currentRatingInput = parseInt(this.getAttribute('data-value')) || 5;
                    updateStarUI(currentRatingInput);
                });

                star.addEventListener('mouseenter', function() {
                    const hoverVal = parseInt(this.getAttribute('data-value')) || 5;
                    updateStarUI(hoverVal);
                });
            });

            const starContainer = document.getElementById('ratingStars');
            if (starContainer) {
                starContainer.addEventListener('mouseleave', function() {
                    updateStarUI(currentRatingInput);
                });
            }
        }

        function updateStarUI(val) {
            const stars = document.querySelectorAll('#ratingStars .star-btn');
            stars.forEach((s, idx) => {
                if (idx < val) {
                    s.classList.remove('text-zinc-700');
                    s.classList.add('text-amber-400');
                } else {
                    s.classList.remove('text-amber-400');
                    s.classList.add('text-zinc-700');
                }
            });
            document.getElementById('ratingScoreText').textContent = val.toFixed(1) + " คะแนน";
        }

        function checkAdminStatusUI() {
            const adminBtnText = document.getElementById('adminBtnText');
            const adminToggleBtn = document.getElementById('adminToggleBtn');
            const adminNotice = document.getElementById('adminNotice');

            if (isAdmin) {
                adminBtnText.textContent = "ตั้งค่าร้านค้า (Admin Mode)";
                adminToggleBtn.classList.remove('bg-zinc-900', 'text-zinc-300');
                adminToggleBtn.classList.add('bg-white', 'text-black', 'font-bold');
                adminNotice.classList.remove('hidden');
            } else {
                adminBtnText.textContent = "โหมดหลังบ้าน (Admin)";
                adminToggleBtn.classList.remove('bg-white', 'text-black', 'font-bold');
                adminToggleBtn.classList.add('bg-zinc-900', 'text-zinc-300');
                adminNotice.classList.add('hidden');
            }
        }

        function toggleAdminAuthModal() {
            if (isAdmin) {
                openAdminPanel();
            } else {
                document.getElementById('adminAuthModal').classList.remove('hidden');
            }
        }

        function closeAdminAuthModal() {
            document.getElementById('adminAuthModal').classList.add('hidden');
        }

        function handleAdminAuth(e) {
            e.preventDefault();
            const pwdInput = document.getElementById('adminPassword');
            const pwd = pwdInput ? pwdInput.value.trim() : '';

            if (pwd === "1234" || pwd === "admin") {
                isAdmin = true;
                sessionStorage.setItem('defy_admin_session', 'true');
                closeAdminAuthModal();
                pwdInput.value = '';
                
                checkAdminStatusUI();
                renderReviews();
                openAdminPanel();
                showToast("เข้าสู่โหมดหลังบ้านสำเร็จ!");
            } else {
                showToast("รหัสผ่านไม่ถูกต้อง!", "error");
            }
        }

        function logoutAdmin() {
            isAdmin = false;
            sessionStorage.removeItem('defy_admin_session');
            closeAdminPanel();
            checkAdminStatusUI();
            renderReviews();
            showToast("ออกจากระบบหลังบ้านแล้ว");
        }

        function openAdminPanel() {
            document.getElementById('editStoreName').value = storeData.name || '';
            document.getElementById('editStoreDesc').value = storeData.desc || '';
            document.getElementById('editProfileUrl').value = storeData.profileUrl || '';
            document.getElementById('editCoverUrl').value = storeData.coverUrl || '';
            document.getElementById('editLineId').value = storeData.lineId || '';
            document.getElementById('editFb').value = storeData.fb || '';
            document.getElementById('editAccountBannerUrl').value = storeData.accountBannerUrl || '';

            document.getElementById('adminPanelModal').classList.remove('hidden');
        }

        function closeAdminPanel() {
            document.getElementById('adminPanelModal').classList.add('hidden');
        }

        function saveAdminSettings() {
            storeData.name = document.getElementById('editStoreName').value.trim() || DEFAULT_STORE_DATA.name;
            storeData.desc = document.getElementById('editStoreDesc').value.trim();
            storeData.profileUrl = document.getElementById('editProfileUrl').value.trim() || DEFAULT_STORE_DATA.profileUrl;
            storeData.coverUrl = document.getElementById('editCoverUrl').value.trim() || DEFAULT_STORE_DATA.coverUrl;
            storeData.lineId = document.getElementById('editLineId').value.trim() || DEFAULT_STORE_DATA.lineId;
            storeData.fb = document.getElementById('editFb').value.trim() || DEFAULT_STORE_DATA.fb;
            storeData.accountBannerUrl = document.getElementById('editAccountBannerUrl').value.trim() || DEFAULT_STORE_DATA.accountBannerUrl;

            if (safeSaveToStorage('defy_store_data_v2', storeData)) {
                renderStoreData();
                closeAdminPanel();
                showToast("บันทึกการปรับแต่งร้านค้าเรียบร้อยแล้ว!");
            }
        }

        function promptOwnerReply(id) {
            const target = reviews.find(r => r.id === id);
            if (!target) return;

            const existingReply = target.reply || '';
            const replyMsg = prompt("กรอกข้อความตอบกลับของทางร้าน:", existingReply);
            
            if (replyMsg !== null) {
                target.reply = replyMsg.trim();
                safeSaveToStorage('defy_store_reviews_v2', reviews);
                renderReviews();
                showToast("บันทึกคำตอบกลับแล้ว");
            }
        }

        function handleReviewSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('reviewerName').value.trim();
            const avatar = document.getElementById('reviewerAvatar').value.trim();
            const comment = document.getElementById('reviewerComment').value.trim();
            const proofUrl = document.getElementById('reviewerProof').value.trim();

            if (!name || !comment) {
                showToast("กรุณากรอกข้อมูลให้ครบถ้วน", "error");
                return;
            }

            const today = new Date();
            const dateStr = today.toLocaleDateString('th-TH', { day: 'numeric', month: 'ส.ค.', year: 'numeric' });

            const newReview = {
                id: Date.now(),
                name: name,
                rating: currentRatingInput,
                comment: comment,
                date: dateStr,
                avatar: avatar || `https://api.dicebear.com/7.x/bottts/svg?seed=${encodeURIComponent(name)}`,
                proofUrl: proofUrl || "",
                reply: ""
            };

            reviews.unshift(newReview);
            if (safeSaveToStorage('defy_store_reviews_v2', reviews)) {
                renderReviews();
                document.getElementById('reviewForm').reset();
                removeAttachedProof();
                document.getElementById('avatarPreviewContainer').classList.add('hidden');
                currentRatingInput = 5;
                updateStarUI(5);
                showToast("ส่งความคิดเห็นและแนบภาพเรียบร้อยแล้ว!");
            }
        }

        function removeAttachedProof() {
            document.getElementById('reviewerProof').value = '';
            document.getElementById('proofPreview').src = '';
            document.getElementById('proofPreviewContainer').classList.add('hidden');
        }

        function deleteReview(id) {
            if (confirm("คุณแน่ใจหรือไม่ว่าต้องการลบคอมเมนต์นี้?")) {
                reviews = reviews.filter(r => r.id !== id);
                safeSaveToStorage('defy_store_reviews_v2', reviews);
                renderReviews();
                showToast("ลบคอมเมนต์เรียบร้อยแล้ว", "success");
            }
        }

        function safeSaveToStorage(key, value) {
            try {
                localStorage.setItem(key, JSON.stringify(value));
                return true;
            } catch (err) {
                console.error("Storage quota error:", err);
                showToast("ไม่สามารถบันทึกข้อมูลได้ เนื่องจากไฟล์ภาพมีขนาดใหญ่เกินไป", "error");
                return false;
            }
        }

        function handleFileUpload(event, targetInputId, previewImgId = null) {
            const file = event.target.files[0];
            if (!file) return;

            // Maximum file limit ~3MB to prevent localStorage crashes
            if (file.size > 3 * 1024 * 1024) {
                showToast("ขนาดไฟล์ภาพต้องไม่เกิน 3MB", "error");
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const result = e.target.result;
                document.getElementById(targetInputId).value = result;

                if (previewImgId) {
                    const img = document.getElementById(previewImgId);
                    if (img) img.src = result;
                    
                    if (previewImgId === 'proofPreview') {
                        document.getElementById('proofPreviewContainer').classList.remove('hidden');
                    }
                    if (previewImgId === 'avatarPreview') {
                        const avatarContainer = document.getElementById('avatarPreviewContainer');
                        avatarContainer.classList.remove('hidden');
                        avatarContainer.classList.add('flex');
                    }
                }
                showToast("อัปโหลดรูปภาพสำเร็จ!");
            };
            reader.onerror = function() {
                showToast("เกิดข้อผิดพลาดในการอ่านไฟล์ภาพ", "error");
            };
            reader.readAsDataURL(file);
        }

        function resetDataToDefault() {
            if (confirm("ต้องการรีเซ็ตข้อมูลทั้งหมดกลับเป็นค่าเริ่มต้นหรือไม่?")) {
                storeData = { ...DEFAULT_STORE_DATA };
                reviews = [ ...DEFAULT_REVIEWS ];
                localStorage.removeItem('defy_store_data_v2');
                localStorage.removeItem('defy_store_reviews_v2');
                renderStoreData();
                renderReviews();
                closeAdminPanel();
                showToast("รีเซ็ตเป็นค่าเริ่มต้นเรียบร้อย");
            }
        }

        function copyToClipboard(elementId, labelName) {
            const el = document.getElementById(elementId);
            if (!el) return;
            const text = el.textContent;
            
            if (navigator.clipboard && window.isSecureContext) {
                navigator.clipboard.writeText(text).then(() => {
                    showToast(`คัดลอก ${labelName}: "${text}" เรียบร้อย!`);
                }).catch(() => fallbackCopy(text, labelName));
            } else {
                fallbackCopy(text, labelName);
            }
        }

        function fallbackCopy(text, labelName) {
            const tempInput = document.createElement('textarea');
            tempInput.value = text;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);
            showToast(`คัดลอก ${labelName}: "${text}" เรียบร้อย!`);
        }

        function openImageModal(imgSrc, caption = "ป้ายบัญชีและช่องทางชำระเงิน (อัตราส่วน 16:9)") {
            document.getElementById('modalPreviewImg').src = imgSrc;
            document.getElementById('modalCaption').textContent = caption;
            document.getElementById('imageModal').classList.remove('hidden');
        }

        function closeImageModal() {
            document.getElementById('imageModal').classList.add('hidden');
        }

        function showToast(message, type = "success") {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');

            toastMsg.textContent = message;
            if (type === "error") {
                toastIcon.className = "fa-solid fa-circle-xmark text-rose-400 text-base";
            } else {
                toastIcon.className = "fa-solid fa-circle-check text-white text-base";
            }

            toast.classList.remove('translate-y-24', 'opacity-0');
            
            setTimeout(() => {
                toast.classList.add('translate-y-24', 'opacity-0');
            }, 3000);
        }

        function escapeHtml(str) {
            if (!str) return '';
            return String(str)
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
