<template>
  <div>
    <div v-if="loadingSubmit">
      <LoadingSubmit />
    </div>
    <ValidationObserver v-slot="{ handleSubmit }">
      <form
        @submit.prevent="handleSubmit(confirmSubmit)"
        enctype="multipart/form-data"
      >
        <ValidationProvider
          name="Nama Tim"
          rules="required"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Nama Tim *</label>
            <input
              id="namaTim"
              type="text"
              class="form-control"
              v-model="namaTim"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Nama Ketua Tim"
          rules="required"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Nama Ketua Tim *</label>
            <input
              id="namaKetua"
              type="text"
              class="form-control"
              v-model="namaKetua"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="Nama Anggota 1" v-slot="{ errors }">
          <div class="form-group">
            <label>Nama Anggota 1 **</label>
            <input
              id="namaAnggota1"
              type="text"
              class="form-control"
              v-model="namaAnggota1"
              v-on:input="anggota1Available()"
              placeholder="Kosongkan jika tidak ada anggota 1"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="Nama Anggota 2" v-slot="{ errors }">
          <div class="form-group">
            <label>Nama Anggota 2 **</label>
            <input
              id="namaAnggota2"
              type="text"
              class="form-control"
              v-model="namaAnggota2"
              v-on:input="anggota2Available()"
              placeholder="Kosongkan jika tidak ada anggota 2"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Nomor HP Ketua"
          rules="required|numeric"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Nomor HP Ketua *</label>
            <input
              id="hpKetua"
              type="text"
              class="form-control"
              v-model="hpKetua"
              placeholder="contoh : 081234567890"
            />
            <br />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Nomor WhatsApp Ketua"
          rules="required|numeric"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Nomor WhatsApp Ketua *</label>
            <input
              id="waKetua"
              type="text"
              class="form-control"
              v-model="waKetua"
              placeholder="contoh : 081234567890"
            />
            <br />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="ID Line" rules="required" v-slot="{ errors }">
          <div class="form-group">
            <label>ID Line *</label>
            <input
              id="lineKetua"
              type="text"
              class="form-control"
              v-model="lineKetua"
              placeholder="Tulis (-) Jika Tidak Ada"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider name="Kota" rules="required" v-slot="{ errors }">
          <div class="form-group">
            <label>Kota *</label>
            <input
              id="asalKota"
              type="text"
              class="form-control"
              v-model="asalKota"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Nama Sekolah / Instansi "
          rules="required"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Nama Sekolah / Instansi *</label>
            <input
              id="asalInstansi"
              type="text"
              class="form-control"
              v-model="asalInstansi"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Alamat Sekolah / Instansi"
          rules="required"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Alamat Sekolah / Instansi *</label>
            <input
              id="alamatInstansi"
              type="text"
              class="form-control"
              v-model="alamatInstansi"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Asal Informasi Mage 7"
          rules="required"
          v-slot="{ errors }"
        >
          <div class="form-group">
            <label>Darimana Anda Tahu Informasi Mengenai MAGE 7 *</label>
            <input
              id="asalInfo"
              type="text"
              class="form-control"
              v-model="asalInfo"
              placeholder="contoh : twitter, instagram dll"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Foto Identitas Ketua Tim"
          rules="required|ext:jpeg,jpg,png,pdf|size:2048"
          v-slot="{ validate, errors }"
        >
          <div class="form-group">
            <label
              >Foto Identitas Ketua Tim (KTP atau SIM atau yang lain & File Max
              2 MB) *</label
            >
            <img
              v-if="previewImage1"
              :src="previewImage1"
              class="uploading-image"
            />
            <input
              type="file"
              accept="image/*"
              class="form-control"
              @change="
                {
                  onUpload1($event) || validate($event);
                }
              "
              id="identitasKetua"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Foto Identitas Anggota 1"
          rules="ext:jpeg,jpg,png,pdf|size:2048"
          v-slot="{ validate, errors }"
          id="identitasAnggota1"
        >
          <div class="form-group">
            <label
              >Foto Identitas Anggota 1 (KTP atau SIM atau yang lain, Kosongkan
              jika tidak ada anggota 1 & File Max 2 MB) *</label
            >
            <img
              v-if="previewImage2"
              :src="previewImage2"
              class="uploading-image"
            />
            <input
              type="file"
              accept="image/*"
              class="form-control"
              @change="
                {
                  onUpload2($event) || validate($event);
                }
              "
              id="identitasAnggota1"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <ValidationProvider
          name="Foto Identitas Anggota 2"
          rules="ext:jpeg,jpg,png,pdf|size:2048"
          v-slot="{ validate, errors }"
          id="identitasAnggota2"
        >
          <div class="form-group">
            <label
              >Foto Identitas Anggota 2 (KTP atau SIM atau yang lain, Kosongkan
              jika tidak ada anggota 2 & File Max 2 MB) *</label
            >
            <img
              v-if="previewImage3"
              :src="previewImage3"
              class="uploading-image"
            />
            <input
              type="file"
              accept="image/*"
              class="form-control"
              @change="
                {
                  onUpload3($event) || validate($event);
                }
              "
              id="identitasAnggota2"
            />
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider>

        <!-- <ValidationProvider name="Surat Keterangan Siswa" rules="ext:jpeg,jpg,png,pdf" v-slot="{ validate, errors }" id="suratKeteranganSiswa">
          <div class="form-group">
            <label>Surat Keterangan Siswa (Kosongkan jika anda kategori Mahasiswa) *</label>
            <img v-if="previewImage4" :src="previewImage4" class="uploading-image" />
            <input type="file" accept="image/*,application/pdf" class="form-control" @change="{ onUpload4($event) || validate($event) }">
            <span class="error-msg">{{ errors[0] }}</span>
          </div>
        </ValidationProvider> -->

        <div>
          <p class="mt-4">Keterangan :</p>
          <p>- Tanda (*) Wajib Di Isi</p>
          <p>- Tanda (**) Opsional</p>
          <p>- Semua file yang diupload maksimal 2 MB</p>
        </div>
        <input type="submit" class="btn btn-red" tect="Submit" />
      </form>
    </ValidationObserver>
  </div>
</template>

<script>
import { ValidationProvider } from "vee-validate/dist/vee-validate.full.esm";
import { ValidationObserver } from "vee-validate";
import Swal from "sweetalert2";
import axios from "axios";
import LoadingSubmit from "@/components/LoadingSubmit";

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
    LoadingSubmit,
  },
  data() {
    return {
      loadingSubmit: false,
      previewImage1: null,
      previewImage2: null,
      previewImage3: null,
      previewImage4: null,
      namaTim: "",
      namaKetua: "",
      waKetua: "",
      lineKetua: "",
      hpKetua: "",
      namaAnggota1: "",
      namaAnggota2: "",
      asalKota: "",
      asalInstansi: "",
      asalInfo: "",
      identitasKetua: null,
      identitasAnggota1: null,
      identitasAnggota2: null,
      // suratKeteranganSiswa: null,
      alamatInstansi: "",
      id: "",
    };
  },
  mounted() {
    document.getElementById("identitasAnggota1").style.display = "none";
    document.getElementById("identitasAnggota2").style.display = "none";
  },
  methods: {
    // isMahasiswa() {
    //   if (this.kategori == "Siswa") {
    //     document.getElementById("suratKeteranganSiswa").style.display = "block"
    //   } else {
    //     document.getElementById("suratKeteranganSiswa").style.display = "none"
    //   }
    // },
    anggota1Available() {
      if (this.namaAnggota1 == null || this.namaAnggota1 == "") {
        document.getElementById("identitasAnggota1").style.display = "none";
      } else {
        document.getElementById("identitasAnggota1").style.display = "block";
      }
    },
    anggota2Available() {
      if (this.namaAnggota2 == null || this.namaAnggota2 == "") {
        document.getElementById("identitasAnggota2").style.display = "none";
      } else {
        document.getElementById("identitasAnggota2").style.display = "block";
      }
    },
    onUpload1(e) {
      this.identitasKetua = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(this.identitasKetua);
      reader.onload = (e) => {
        this.previewImage1 = e.target.result;
      };
    },
    onUpload2(e) {
      this.identitasAnggota1 = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(this.identitasAnggota1);
      reader.onload = (e) => {
        this.previewImage2 = e.target.result;
      };
    },
    onUpload3(e) {
      this.identitasAnggota2 = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(this.identitasAnggota2);
      reader.onload = (e) => {
        this.previewImage3 = e.target.result;
      };
    },
    onUpload4(e) {
      this.suratKeteranganSiswa = e.target.files[0];
      const reader = new FileReader();
      reader.readAsDataURL(this.suratKeteranganSiswa);
      reader.onload = (e) => {
        this.previewImage4 = e.target.result;
      };
    },
    onSubmit() {
      const document = new FormData();
      this.loadingSubmit = true;
      if (this.identitasAnggota1 == null || this.identitasAnggota1 == "") {
        console.log("Tidak Ada identitas anggota 1");
      } else {
        document.append("identitasAnggota1", this.identitasAnggota1);
      }

      if (this.identitasAnggota2 == null || this.identitasAnggota2 == "") {
        console.log("Tidak Ada identitas anggota 1");
      } else {
        document.append("identitasAnggota2", this.identitasAnggota2);
      }
      if (this.namaAnggota1 == null || this.namaAnggota1 == "") {
        console.log("Tidak Ada anggota 1");
      } else {
        document.append("namaAnggota1", this.namaAnggota1);
      }

      if (this.namaAnggota2 == null || this.namaAnggota2 == "") {
        console.log("Tidak Ada anggota 2");
      } else {
        document.append("namaAnggota2", this.namaAnggota2);
      }

      // if(this.kategori == 'Siswa') {
      //   document.append("suratKeteranganSiswa", this.suratKeteranganSiswa);
      // }
      document.append("identitasKetua", this.identitasKetua);
      document.append("namaTim", this.namaTim);
      document.append("namaKetua", this.namaKetua);
      document.append("waKetua", this.waKetua);
      document.append("lineKetua", this.lineKetua);
      document.append("hpKetua", this.hpKetua);
      document.append("asalKota", this.asalKota);
      document.append("asalInstansi", this.asalInstansi);
      document.append("asalInfo", this.asalInfo);
      document.append("alamatInstansi", this.alamatInstansi);

      const formData = {
        data: document,
      };
      this.$store.dispatch("regisCompetition/registerOlim", formData).then(
        () => {
          this.loadingSubmit = false;
          Swal.fire({
            icon: "success",
            title: "Register berhasil",
            showConfirmButton: true,
          }).then(() => {
            this.$router.push("/dashboard");
          });
        },
        (error) => {
          if (error.response.data.code == 401) {
            this.refreshToken();
          } else {
            this.loadingSubmit = false;
            Swal.fire({
              icon: "error",
              title: "Register gagal",
              text: error.response.data.message,
              showConfirmButton: true,
            }).then(() => {});
          }
        }
      );
    },
    refreshToken() {
      const user = JSON.parse(localStorage.getItem("user"));
      axios
        .post(`${this.endpointAPI}api/v1/auth/refresh-tokens`, {
          refreshToken: user.tokens.refresh.token,
        })
        .then((response) => {
          user.tokens = response.data;
          localStorage.setItem("user", JSON.stringify(user));
        })
        .then(() => {
          this.onSubmit();
        });
    },
    confirmSubmit() {
      Swal.fire({
        icon: "warning",
        title: "Anda Yakin Data yang Anda Isikan Sudah Benar ?",
        showDenyButton: true,
        confirmButtonText: "Yes",
        denyButtonText: "No",
      }).then((result) => {
        if (result.isConfirmed) {
          this.onSubmit();
        }
      });
    },
  },
};
</script>

<style scoped>
.error-msg {
  color: red;
}

.uploading-image {
  display: flex;
}
p {
  margin-top: -20px;
  font-size: 13px;
}
img {
  width: 200px;
  height: auto;
}
</style>
