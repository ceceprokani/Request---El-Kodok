<template>
  <div class="container p-3 text-white">
    <div class="d-flex my-3 justify-content-between">
      <div class="d-flex align-items-start text-warning flex-shrink-0 me-3">
        <img src="@/assets/images/logo.png" class="me-3" style="height: 50px;">
        <div class="custom-rounded-medium text-dark px-3 py-1 bg-warning h5 fw-bold ms-2">EL KODOK GAMING</div>
      </div>
      <div class="d-flex align-items-center">
        <button type="button" class="btn btn-warning custom-rounded fw-bold mobile-hide" data-bs-toggle="modal" data-bs-target="#addData" v-if="listData.length"><i class="mdi mdi-plus me-2"></i>TAMBAH REQUEST</button>
        <div class="dropdown flex-shrink-1 mobile-hide">
          <button type="button" class="btn btn-outline-warning custom-rounded ms-2" id="dropdownMenuButton" data-bs-toggle="dropdown" aria-expanded="false"><i class="mdi mdi-dots-vertical ml-1 p-0"></i></button>
          <ul class="dropdown-menu custom-rounded" aria-labelledby="dropdownMenuButton">
            <li><a class="dropdown-item" href="javascript:void(0)" data-bs-toggle="modal" data-bs-target="#gameRules">Game Rules</a></li>
          </ul>
        </div>
        <div class="dropdown mobile-show">
          <button class="btn btn-warning custom-rounded d-flex align-items-center justify-content-center" type="button" id="dropdownMenuButton" data-bs-toggle="dropdown" aria-expanded="false">
            <i class="mdi mdi-dots-vertical fs-2 ml-1 p-0"></i>
          </button>
          <ul class="dropdown-menu custom-rounded" aria-labelledby="dropdownMenuButton">
            <!-- <li><a class="dropdown-item" href="javascript:void(0)" data-bs-toggle="modal" data-bs-target="#addData" @click="resetForm" >Add New Player</a></li>
            <li><a class="dropdown-item" href="javascript:void(0)" data-bs-toggle="modal" data-bs-target="#gameRules">Game Rules</a></li> -->
          </ul>
        </div>
      </div>
    </div>
    <div class="my-3">&nbsp;</div>
    <div v-if="fetching" class="d-flex align-items-center justify-content-center h-75">
      <div class="d-block text-center">
        <img src="@/assets/images/ripples.svg" class="mb-3" style="width: 100px;">
        <div>LOADING ...</div>
      </div>
    </div>
    <template v-else>
      <template v-if="listData.length || listDataTwo.length"> 
        <div class="d-block bg-secondary custom-rounded mb-3 bg-texture">
          <div class="d-flex align-items-start justify-content-between fw-bold mb-0 bg-gradient p-4 custom-rounded-card-header">
            <div class="d-flex align-items-center">
              <img src="@/assets/images/shuttle.png" width="32" class="mx-2" />
              <div class="ms-2 h5 m-0">On Progress</div>
            </div>
            <div>{{ listData.length }} Request</div>
          </div>
          <div class="row mx-3 mb-4">
            <draggable
              :list="listData"
              :item-key="'id'"
              class="draggable-list my-3"
              :tag="'div'"
              direction="horizontal"
              group="request"
              animation="300">
              <template #item="{ element, index }">
                <div class="col-md-3 draggable-item my-2" v-if="index < (listData.length + 1)">
                  <div :class="`card custom-rounded border-0 bg-dark player ${index == 0 ? 'win-effect' : ''}`">
                    <div class="card-header custom-rounded-card-header bg-secondary p-0 border-0">
                      <div class="d-flex justify-content-between align-items-center custom-rounded-card-header" :style="`background: url(${element.hero}); height: 200px; width: 100%; background-size: cover; background-position: center`">
                      </div>
                    </div>
                    <div class="card-body">
                      <div class="d-flex justify-content-between">
                        <div class="d-block">
                          <h4 class="text-warning fw-bold">{{ element.hero_name }}</h4>
                          <div>{{ element.commander }} <span class="badge border border-white custom-rounded ms-1" style="font-size: 9pt">Skill : {{ element.commander_skill }}</span></div>
                          <div class="fw-bold mb-3">{{ element.name }}</div>
                          <div class="badge bg-secondary">Percobaan 1/3</div>
                        </div>
                        <div class="d-flex flex-column justify-content-between align-items-end">
                          <div class="d-flex" style="gap: 4px">
                            <template v-for="(item, pos) in element.emblem">
                              <div :style="{ backgroundImage: `url('${item}')`, width: '40px', height: '40px', backgroundSize: 'contain', backgroundRepeat: 'no-repeat', boxShadow: '0 4px 4px rgba(0, 0, 0, 0.3)', objectFit: 'cover', borderRadius: '50%', backgroundColor: backgroundColor[index + pos] }" ></div>
                            </template>
                          </div>
                          <div class="text-warning fw-bold" v-if="element.is_vip">
                            <img src="@/assets/images/flash.png" style="width: 30px; height: 30px;" class="img-fluid"> VIP
                          </div>
                        </div>
                      </div>
                    </div>
                    <div class="card-footer">
                      <div class="d-grid pt-2 mb-2">
                        <button type="button" class="btn btn-warning custom-rounded fw-bold show-hover" data-bs-toggle="modal" data-bs-target="#formScore" @click="openFormScore(player)">SELESAIKAN</button>
                      </div>
                    </div>
                  </div>
                </div>
              </template>
              <template #footer>
                <div v-if="listData.length === 0" class="d-flex align-items-center justify-content-center custom-rounded w-100 text-center" style="min-height: 100px; background-color: rgba(0, 0, 0, 0.5);">
                  <i class="mdi mdi-information-outline me-1"></i> Belum ada request yang sedang dikerjakan.
                </div>
              </template>
            </draggable>
          </div>
        </div>
        <div class="d-block bg-secondary custom-rounded mb-3 bg-texture-2">
          <div class="d-flex align-items-start justify-content-between fw-bold mb-0 bg-gradient p-4 custom-rounded-card-header">
            <div class="d-flex align-items-center">
              <img src="@/assets/images/stack.png" width="32" class="mx-2" />
              <div class="ms-2 h5 m-0">Daftar Antrian</div>
            </div>
            <div>{{ listDataTwo.length }} Request</div>
          </div>
          <div class="row mx-3 mb-4">
              <draggable
                :list="listDataTwo"
                :item-key="'id'"
                class="draggable-list my-3"
                :tag="'div'"
                group="request"
                direction="horizontal"
                animation="300">
                <template #item="{ element, index }">
                  <div class="col-md-3 draggable-item my-2">
                    <div :class="`card custom-rounded border-0 bg-dark player`">
                      <div class="card-header custom-rounded-card-header bg-secondary p-0 border-0">
                        <div class="d-flex justify-content-between align-items-center custom-rounded-card-header" :style="`background: url(${element.hero}); height: 200px; width: 100%; background-size: cover; background-position: center`">
                        </div>
                      </div>
                      <div class="card-body">
                        <div class="d-flex justify-content-between">
                          <div class="d-block">
                            <h4 class="text-warning fw-bold">{{ element.hero_name }}</h4>
                            <div>{{ element.commander }} <span class="badge border border-white custom-rounded ms-1" style="font-size: 9pt">Skill : {{ element.commander_skill }}</span></div>
                            <div class="fw-bold mb-3">{{ element.name }}</div>
                            <!-- <div class="badge bg-secondary">Percobaan 1/3</div> -->
                          </div>
                          <div class="d-flex flex-column justify-content-between align-items-end">
                            <div class="d-flex" style="gap: 4px">
                              <template v-for="(item, pos) in element.emblem">
                                <div :style="{ backgroundImage: `url('${item}')`, width: '40px', height: '40px', backgroundSize: 'contain', backgroundRepeat: 'no-repeat', boxShadow: '0 4px 4px rgba(0, 0, 0, 0.3)', objectFit: 'cover', borderRadius: '50%', backgroundColor: backgroundColor[index + pos] }" ></div>
                              </template>
                            </div>
                            <div class="text-warning fw-bold" v-if="element.is_vip">
                              <img src="@/assets/images/flash.png" style="width: 30px; height: 30px;" class="img-fluid"> VIP
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </template>
                <template #footer>
                  <div v-if="listDataTwo.length === 0" class="d-flex align-items-center justify-content-center bg-dark custom-rounded w-100 text-center" style="min-height: 100px">
                    <i class="mdi mdi-information-outline me-1"></i> Belum ada request yang sedang dikerjakan.
                  </div>
                </template>
              </draggable>
          </div>
        </div>
        <div class="spacer-medium">&nbsp;</div>
        <div class="d-flex justify-content-between">
          <div class="d-block mb-3 h4">Daftar Request Selesai</div>
          <div>4 Request</div>
        </div>
        <div class="table-responsive">
          <table class="table table-custom-card-dark">
            <thead>
              <tr>
                <th class="text-center">#</th>
                <th>Nama</th>
                <th>Hero</th>
                <th>Emblem</th>
                <th>Commander</th>
                <th>Tanggal Request</th>
                <th>Tanggal Selesai</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td class="text-center">1</td>
                <td class="fw-bold text-warning">Cecep CauDev <span class="badge bg-warning ms-2 text-dark">VIP</span></td>
                <td>Leomord</td>
                <td>Cadia</td>
                <td>Yuki <span class="badge bg-dark ms-2 text-light">Skill: 2</span></td>
                <td>27 Desember 2024 09:00</td>
                <td>28 Desember 2024 09:00</td>
              </tr>
              <tr>
                <td class="text-center">2</td>
                <td class="fw-bold text-warning">Cecep CauDev</td>
                <td>Leomord</td>
                <td>Cadia</td>
                <td>Yuki <span class="badge bg-dark ms-2 text-light">Skill: 2</span></td>
                <td>27 Desember 2024 09:00</td>
                <td>28 Desember 2024 09:00</td>
              </tr>
              <tr>
                <td class="text-center">3</td>
                <td class="fw-bold text-warning">Cecep CauDev <span class="badge bg-warning ms-2 text-dark">VIP</span></td>
                <td>Leomord</td>
                <td>Cadia</td>
                <td>Yuki <span class="badge bg-dark ms-2 text-light">Skill: 2</span></td>
                <td>27 Desember 2024 09:00</td>
                <td>28 Desember 2024 09:00</td>
              </tr>
              <tr>
                <td class="text-center">4</td>
                <td class="fw-bold text-warning">Cecep CauDev</td>
                <td>Leomord</td>
                <td>Cadia</td>
                <td>Yuki <span class="badge bg-dark ms-2 text-light">Skill: 2</span></td>
                <td>27 Desember 2024 09:00</td>
                <td>28 Desember 2024 09:00</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="spacer-medium">&nbsp;</div>
      </template>
      <div class="row h-75 align-items-center" v-else>
        <div class="col-md-12 text-center">
          <img src="@/assets/images/empty-inbox.png" style="width: 150px;" class="img-fluid mb-3">
          <div class="text-muted">Belum ada request untuk saat ini</div>
          <div class="text-muted mb-4">Klik tombol "Tambah" dibawah untuk menambah request</div>

          <button class="btn btn-warning px-3 custom-rounded fw-bold" data-bs-toggle="modal" data-bs-target="#addData" @click="resetForm"><i class="mdi mdi-plus me-2"></i>TAMBAH REQUEST BARU</button>
        </div>
      </div>
    </template>
  </div>
  <!-- <div class="modal fade" id="confirm" tabindex="-1" aria-labelledby="confirmLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content custom-rounded bg-dark">
        <div class="modal-header border-0">
          <h5 class="modal-title text-white" id="confirmLabel">{{ confirmMessage.title }}</h5>
        </div>
        <div class="modal-body">
          <div class="mb-2 text-white" v-html="confirmMessage.message"></div>
        </div>
        <div class="modal-footer border-0">
          <button type="button" ref="closeModalConfirm" :disabled="fetching" class="btn btn-link text-white text-decoration-none custom-rounded" data-bs-dismiss="modal">Cancel</button>
          <button type="button" class="btn btn-secondary custom-rounded" :disabled="fetching" @click="actionConfirm(confirmMessage.action)">{{ confirmMessage.action_title }}</button>
        </div>
      </div>
    </div>
  </div> -->

  <!-- Modal for game rules -->
  <div class="modal fade" id="gameRules" tabindex="-1" aria-labelledby="gameRulesLabel" aria-hidden="true">
    <div class="modal-dialog modal-fullscreen">
      <div class="modal-content bg-dark">
        <div class="modal-header border-0">
          <div class="row w-100">
            <div class="col-md-6 m-auto">
              <h5 class="modal-title text-white fw-bold" id="gameRulesLabel">PERATURAN</h5>
            </div>
          </div>
        </div>
        <div class="modal-body">
          <div class="row w-100">
            <div class="col-md-6 m-auto">
              <div class="form-group text-white mb-3" style="line-height: 35px; text-align: justify">
                <div>*HARAP PERHATIKAN SAMPAI "ketentuan tambahan"</div>
                <div class="mb-3">*KALO DUIT MASIH PAS-PASAN JANGAN MAKSA REQUEST, KARENA REQUESTAN TIDAK 100% BAKAL BERHASIL. JADI SAYANGI DUIT ANDA WOIII</div>
                <ol>
                  <li class="mb-3 text-warning fw-bold">
                    <span>Paket Kapitalis 150K (PRIORITAS)</span>
                    <ol type="a" class="text-white fw-normal">
                      <li>Sampai mencapai urutan 4 besar selesai (done).</li>
                      <li>Hyper akan diusahakan mencapai bintang 3.</li>
                      <li>Jika gagal, akan dicoba sebanyak 5 kali.</li>
                      <li>Permintaan yang dianggap <span class="text-warning fw-bold">TIDAK MASUK AKAL</span> oleh Jefri Nicho akan <span class="text-warning fw-bold">DIKEMBALIKAN</span> (dipotong biaya admin Sosiabuzz dan bank).</li>
                      <li>Permintaan yang sama tidak akan diulang lebih dari 5 kali, berapa pun nominalnya!</li>
                    </ol>
                  </li>
                  <li class="mb-3 text-warning fw-bold">
                    <span>Paket Murah Meriah 30K</span>
                    <p class="text-white fw-normal">Dapatkan poin tertinggi pada setiap permainan.</p>
                    <ol type="a" class="text-white fw-normal">
                      <li>Permintaan yang dianggap <span class="text-warning fw-bold">TIDAK MASUK AKAL</span> oleh Jefri Nicho akan <span class="text-warning fw-bold">DIKEMBALIKAN</span> (dipotong biaya admin Sosiabuzz dan bank).</li>
                      <li>Hanya diberikan <span class="text-warning fw-bold">1x</span> percobaan.</li>
                    </ol>
                  </li>
                  <li class="mb-3 text-warning fw-bold">
                    <span>Ketentuan Tambahan</span>
                    <ol type="a" class="text-white fw-normal">
                      <li><span class="fw-bold text-warning">Paket Kapitalis</span> akan menggeser antrian <span class="text-warning fw-bold">Paket Murah Meriah</span>.</li>
                      <li><span class="text-warning fw-bold">Paket Murah Meriah</span> akan dikerjakan setelah pengerjaan 2 <span class="fw-bold text-warning">Paket Kapitalis</span> dan seterusnya.</li>
                      <li>Permintaan yang masuk setelah penutupan request akan dikerjakan pada stream berikutnya.</li>
                      <li>Hanya menerima request sesuai ketentuan yang tertulis di atas.</li>
                    </ol>
                  </li>
                </ol>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer border-0">
          <button type="button" :disabled="fetching" class="btn btn-link text-white text-decoration-none custom-rounded" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
// import { max, findIndex, map } from 'lodash';

// import { db } from '@/utils/firebase';
// import { doc, setDoc, getDocs, addDoc, collection, where, deleteDoc, query, orderBy } from "firebase/firestore";

import draggable from "vuedraggable";

import sun from '@/assets/images/heroes/sun.jpg';
import leomord from '@/assets/images/heroes/leomord.jpg';
import texture from '@/assets/images/texture.jpeg';

export default {
  data() {
    return {
      listData: [{
        id: 1,
        name: 'El Temon',
        hero_name: 'Sun',
        commander: 'Yuki',
        commander_skill: 2,
        emblem: [
          new URL('@/assets/images/emblem/asassin.png', import.meta.url).href
        ],
        hero: sun,
        status: 'ongoing',
        is_vip: true,
      },
      {
        id: 2,
        name: 'Agus',
        hero_name: 'Leomord',
        commander: 'Zilong',
        commander_skill: 2,
        emblem: [
          new URL('@/assets/images/emblem/cadia.png', import.meta.url).href,
          new URL('@/assets/images/emblem/marksman.png', import.meta.url).href
        ],
        hero: leomord,
        status: 'ongoing',
        is_vip: true,
      }],
      listDataTwo: [{
        id: 3,
        name: 'El Temon',
        hero_name: 'Sun',
        commander: 'Yuki',
        commander_skill: 2,
        emblem: [
          new URL('@/assets/images/emblem/asassin.png', import.meta.url).href
        ],
        hero: sun,
        status: 'ongoing',
        is_vip: false,
      },
      {
        id: 4,
        name: 'Agus',
        hero_name: 'Leomord',
        commander: 'Zilong',
        commander_skill: 2,
        emblem: [
          new URL('@/assets/images/emblem/cadia.png', import.meta.url).href,
          new URL('@/assets/images/emblem/marksman.png', import.meta.url).href
        ],
        hero: leomord,
        status: 'ongoing',
        is_vip: false,
      },
      {
        id: 5,
        name: 'Agus',
        hero_name: 'Leomord',
        commander: 'Zilong',
        commander_skill: 2,
        emblem: [
          new URL('@/assets/images/emblem/cadia.png', import.meta.url).href,
          new URL('@/assets/images/emblem/marksman.png', import.meta.url).href
        ],
        hero: leomord,
        status: 'ongoing',
        is_vip: false,
      }],
      fetching: false,
      items: [
        { id: 1, name: "Item 1" },
        { id: 2, name: "Item 2" },
      ],
      backgroundColor: [],
    }
  },
  components: {draggable, simplebar},
  computed: {
  },
  mounted() {
    this.listData.forEach((element, parentIndex) => {
      element.emblem.forEach((emblem, index) => {
        this.getDominantColor(parentIndex + index, emblem);
      });
    });
  },
  methods: {
    getDominantColor(index, imageSrc) {
      const img = new Image();
      img.src = imageSrc;
      img.onload = () => {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = img.width;
        canvas.height = img.height;
        ctx.drawImage(img, 0, 0);
        const data = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const pixels = data.data;
        let r = 0, g = 0, b = 0, count = 0;

        for (let i = 0; i < pixels.length; i += 4) {
          r += pixels[i];     // Red
          g += pixels[i + 1]; // Green
          b += pixels[i + 2]; // Blue
          count++;
        }

        r = Math.floor(r / count);
        g = Math.floor(g / count);
        b = Math.floor(b / count);

        this.backgroundColor[index] = `rgb(${r}, ${g}, ${b})`;
      };
    }
  }
}
</script>