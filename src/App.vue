<script setup lang="ts">
import { computed, ref } from "vue";

type Scene = "door" | "room";
type Modal = "puzzle" | "video" | "flower" | "letter" | "coupons" | null;

const scene = ref<Scene>("door");
const modal = ref<Modal>(null);
const doorOpening = ref(false);

const puzzleSolved = ref(false);
const videoWatched = ref(false);
const flowerViewed = ref(false);
const letterOpened = ref(false);

const pieces = ref([3, 1, 6, 8, 2, 7, 4, 9, 5]);
const target = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const isSolved = computed(() => pieces.value.every((p, i) => p === target[i]));

function shufflePuzzle() {
  pieces.value = [...target].sort(() => Math.random() - 0.5);

  if (pieces.value.every((p, i) => p === target[i])) {
    shufflePuzzle();
  }
}

function movePiece(index: number) {
  if (puzzleSolved.value) return;

  const emptyIndex = pieces.value.indexOf(9);

  const row = Math.floor(index / 3);
  const col = index % 3;

  const erow = Math.floor(emptyIndex / 3);
  const ecol = emptyIndex % 3;

  if (Math.abs(row - erow) + Math.abs(col - ecol) !== 1) return;

  const next = [...pieces.value];
  [next[index], next[emptyIndex]] = [next[emptyIndex], next[index]];

  pieces.value = next;

  if (isSolved.value) {
    puzzleSolved.value = true;

    setTimeout(() => {
      modal.value = "coupons";
    }, 650);
  }
}

function giveUp() {
  // จัดเรียงชิ้นส่วนให้ถูกต้องทันที
  pieces.value = [...target];
  puzzleSolved.value = true;

  // เปิดหน้า coupons หลังจากแก้เสร็จ
  setTimeout(() => {
    modal.value = "coupons";
  }, 650);
}

function openGift() {
  modal.value = "puzzle";

  if (!puzzleSolved.value) {
    shufflePuzzle();
  }
}

const openVideo = () => {
  window.open(
    "https://www.instagram.com/p/Dc-Fo0JT4Nk/?utm_source=ig_web_copy_link&stkn=NTc4MTIwNjQ2YQ==",
    "_blank",
  );
};

function openFlower() {
  modal.value = "flower";
  flowerViewed.value = true;
}

function openLetter() {
  modal.value = "letter";
  letterOpened.value = true;
}

function closeModal() {
  modal.value = null;
}

function openDoor() {
  if (doorOpening.value) return;

  doorOpening.value = true;

  // รอ animation ประตูเปิดก่อนเข้า room
  setTimeout(() => {
    scene.value = "room";
    doorOpening.value = false;
  }, 1000);
}
</script>

<template>
  <main class="app">
    <!-- ================= FRONT DOOR ================= -->
    <Transition name="door-scene">
      <section
        v-if="scene === 'door'"
        class="door-scene"
        :class="{ 'is-opening': doorOpening }"
      >
        <div class="sparkles" aria-hidden="true">✦　·　✧　·　✦</div>

        <!-- PHOTO BOOTH -->
        <div class="photo-booth">
          <div class="photo-strip">
            <!-- รูปที่ 1 -->
            <div class="photo-slot">
              <img src="/img/tk2.JPG" alt="Namkhing photo 1" />
            </div>

            <!-- รูปที่ 2 -->
            <div class="photo-slot">
              <img src="/img/tk3.JPG" alt="Namkhing photo 2" />
            </div>

            <!-- รูปที่ 3 -->
            <div class="photo-slot">
              <img src="/img/tk4.JPG" alt="Namkhing photo 3" />
            </div>

            <!-- รูปที่ 4 -->
            <div class="photo-slot">
              <img src="/img/tk5.JPG" alt="Namkhing photo 4" />
            </div>

            <div class="photo-booth-caption">kid version ♡</div>
          </div>
        </div>
        <div class="photo-booth2">
          <div class="photo-strip">
            <!-- รูปที่ 1 -->
            <div class="photo-slot">
              <img src="/img/tk6.JPG" alt="Namkhing photo 1" />
            </div>

            <!-- รูปที่ 2 -->
            <div class="photo-slot">
              <img src="/img/tk7.JPG" alt="Namkhing photo 2" />
            </div>

            <!-- รูปที่ 3 -->
            <div class="photo-slot">
              <img src="/img/tk8.JPG" alt="Namkhing photo 3" />
            </div>

            <!-- รูปที่ 4 -->
            <div class="photo-slot">
              <img src="/img/tk1.JPG" alt="Namkhing photo 4" />
            </div>

            <div class="photo-booth-caption">kid version ♡</div>
          </div>
        </div>

        <!-- DOOR -->
        <div class="door" :class="{ opening: doorOpening }">
          <div class="door-sign">Namkhing's room</div>

          <button
            class="door-handle"
            @click="openDoor"
            aria-label="Open the door"
          >
            ●
          </button>
        </div>

        <div class="girl">
          <span class="birthday-text">Happy Birthday<br />P'namkhing</span>
          <img src="/img/tkp.png" alt="Nankhing" />
        </div>

        <!-- OPEN BUTTON -->
        <button class="open-door-btn" :disabled="doorOpening" @click="openDoor">
          {{ doorOpening ? "Opening..." : "" }}
          <span></span>
        </button>
      </section>
    </Transition>

    <!-- ================= ROOM ================= -->
    <section v-if="scene === 'room'" class="room-scene">
      <div class="room-light"></div>

      <header class="room-header">
        <span>
          {{
            [puzzleSolved, videoWatched, flowerViewed, letterOpened].filter(
              Boolean,
            ).length
          }}/4 discovered
        </span>
      </header>

      <div class="room">
        <div class="window">
          <div class="moon">☾</div>
          <div class="star s1">✦</div>
          <div class="star s2">·</div>
          <div class="star s3">✧</div>
        </div>

        <div class="wall-frame">
          <button
            class="object-button frame-button"
            @click="openVideo"
            aria-label="Open the photo frame"
          >
            <span class="frame-photo">♡</span>
            <span class="object-hint">click me</span>
          </button>
        </div>
      </div>
      <!-- TABLE -->
      <div class="table">
        <div class="table-top">
          <!-- FLOWERS -->
          <button
            class="table-object flower-button"
            @click="openFlower"
            aria-label="Open the flowers"
          >
            <img src="/img/flower.png" alt="Flowers" />
          </button>

          <!-- GIFT -->
          <button
            class="table-object gift-button"
            @click="openGift"
            aria-label="Open the gift"
          >
            <img src="/img/gift.png" alt="Gift" />
          </button>

          <!-- LETTER -->
          <button
            class="table-object letter-button"
            @click="openLetter"
            aria-label="Open the letter"
          >
            <img src="/img/letter.png" alt="Letter" />
          </button>
        </div>
      </div>
    </section>

    <!-- ================= PUZZLE ================= -->
    <Transition name="fade">
      <div v-if="modal === 'puzzle'" class="modal-backdrop">
        <div class="modal puzzle-modal">
          <button class="close" @click="closeModal">×</button>

          <p class="eyebrow">Gift #1</p>

          <h2>Complete the little picture to unlock your surprise.</h2>

          <div class="puzzle" :class="{ 'is-solved': puzzleSolved }">
            <button
              v-for="(piece, index) in pieces"
              :key="index"
              class="piece"
              :class="{ empty: piece === 9 }"
              :disabled="puzzleSolved"
              @click="movePiece(index)"
            >
              <!-- แสดงตัวเลขเฉพาะชิ้นส่วนที่ไม่ใช่ช่องว่าง (เลข 9) -->
              <span v-if="piece !== 9">{{ piece }}</span>
            </button>
          </div>

          <div class="puzzle-actions">
            <div class="puzzle-actions">
              <button class="text-btn" @click="shufflePuzzle">
                Shuffle again
              </button>

              <!-- ปุ่มยอมแพ้ -->
              <button class="primary-btn" @click="giveUp">ยอมแพ้แล้ว 🥺</button>
            </div>
          </div>
        </div>
      </div>
    </Transition>

    <!-- ================= COUPONS ================= -->
    <Transition name="fade">
      <div v-if="modal === 'coupons'" class="modal-backdrop">
        <div class="modal coupon-modal">
          <p class="eyebrow">You did it! ✨</p>

          <h2>Three little coupons for you.</h2>

          <p class="modal-subtitle">เริ่มใช้ได้ตอนนี้หมดเขตปีหน้า</p>

          <div class="coupons">
            <article class="coupon">
              <span>01</span>
              <strong>ขอหมอนวดหนึ่งอัตราตอนนี้</strong>
              <img class="coupon-img" src="/img/c1.png" />
            </article>

            <article class="coupon">
              <span>02</span>
              <strong>ขอให้เค้าจ่ายตังให้ เอาบัตรพี่ไปรูดเลยคับ!</strong>
              <img class="coupon-img" src="/img/c2.png" />
            </article>

            <article class="coupon">
              <span>03</span>
              <strong>ขอให้เค้าทำอะไรให้ก็ได้ นางฟ้าประทานพร</strong>
              <img class="coupon-img" src="/img/c3.png" />
            </article>
          </div>

          <button class="primary-btn" @click="closeModal">close</button>
        </div>
      </div>
    </Transition>

    <!-- ================= VIDEO ================= -->
    <Transition name="fade">
      <div v-if="modal === 'video'" class="modal-backdrop">
        <div class="modal media-modal">
          <button class="close" @click="closeModal">×</button>

          <p class="eyebrow">Gift #2</p>

          <h2>A little something I made for you.</h2>

          <div class="video-placeholder">
            <div>
              <span class="play-icon"> ▶ </span>

              <p>YOUR VIDEO GOES HERE</p>

              <small>
                Replace this area with your edited birthday video.
              </small>
            </div>
          </div>

          <button class="primary-btn" @click="closeModal">
            Back to the room
          </button>
        </div>
      </div>
    </Transition>

    <!-- ================= FLOWER ================= -->
    <Transition name="fade">
      <div v-if="modal === 'flower'" class="modal-backdrop">
        <div class="modal flower-modal">
          <button class="close" @click="closeModal">×</button>

          <p class="eyebrow">Gift #3</p>

          <h3>
            ดอกไม้อันนี้มาจากคำว่ามีความสุขมากๆ<br />ขอดูหลักฐานได้ที่คนสวยข้างๆ
          </h3>

          <div class="flower-reveal">
            <div class="bouquet-placeholder">
              <img src="/img/jar.png" />
            </div>
          </div>

          <p class="reveal-text">A little bouquet I made for you. ♡</p>

          <button class="primary-btn" @click="closeModal">
            Back to the room
          </button>
        </div>
      </div>
    </Transition>

    <!-- ================= LETTER ================= -->
    <Transition name="fade">
      <div v-if="modal === 'letter'" class="modal-backdrop letter-backdrop">
        <div class="paper">
          <button class="close paper-close" @click="closeModal">×</button>
          <img src="/img/letter_inside.png" />
        </div>
      </div>
    </Transition>
  </main>
</template>
