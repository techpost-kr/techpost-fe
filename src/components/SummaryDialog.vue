<template>
  <div v-if="isVisible" class="dialog-overlay" @click="closeDialog">
    <div class="dialog-content" @click.stop>
      <div class="dialog-header">
        <button class="close-btn" @click="closeDialog">&times;</button>
      </div>

      <div class="dialog-body">
        <div class="post-info">
          <h2 class="dialog-title">{{ post.title }}</h2>
          <div class="post-meta">
            <div class="company-info">
              <img :src="getCompanyLogo(post.techBlogEnum)" alt="Company Logo" class="company-logo" />
              <span class="company-name">{{ post.techBlogEnum }}</span>
            </div>
            <time class="publish-date">{{ formatDate(post.publishedDateTime) }}</time>
          </div>
        </div>

        <div class="summary-section">
          <h3>요약</h3>
          <div class="summary-content">
            {{ post.summary || '요약 정보가 없습니다.' }}
          </div>
        </div>

        <div class="dialog-actions">
          <button class="original-btn" @click="openOriginal">원문보기</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {defineComponent, type PropType} from 'vue';
import {formatDate} from '@/common/utils/dateUtils';
import kakaoLogo from '@/assets/company/logo/kakao.png';
import naverLogo from '@/assets/company/logo/naver.png';
import type {Post} from "@/types/Post";

export default defineComponent({
  name: 'SummaryDialog',
  props: {
    isVisible: {
      type: Boolean,
      required: true
    },
    post: {
      type: Object as PropType<Post>,
      required: true
    }
  },
  emits: ['close'],
  methods: {
    formatDate,
    closeDialog() {
      this.$emit('close');
    },
    openOriginal() {
      window.open(this.post.url, '_blank');
      this.closeDialog()
    },
    getCompanyLogo(techBlogEnum: string) {
      const logoMap: { [key: string]: string } = {
        KAKAO: kakaoLogo,
        NAVER: naverLogo,
      };
      return logoMap[techBlogEnum] || '@/assets/logo.svg';
    }
  }
});
</script>

<style scoped>
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.dialog-content {
  background: white;
  border-radius: 12px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
  font-family: 'Spoqa Han Sans', sans-serif;
}

.dialog-header {
  display: flex;
  justify-content: flex-end;
  padding: 15px 20px 0;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
  padding: 0;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  background-color: #f5f5f5;
  color: #333;
}

.dialog-body {
  padding: 0 20px 20px;
}

.post-info {
  margin-bottom: 25px;
}

.dialog-title {
  font-size: 1.4em;
  font-weight: 600;
  margin: 0 0 15px 0;
  line-height: 1.4;
  color: #333;
}

.post-meta {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 10px;
}

.company-info {
  display: flex;
  align-items: center;
}

.company-logo {
  width: 20px;
  height: auto;
  margin-right: 8px;
}

.company-name {
  font-size: 0.9em;
  color: #666;
  font-weight: 500;
}

.publish-date {
  font-size: 0.9em;
  color: #666;
}

.summary-section {
  margin-bottom: 25px;
}

.summary-section h3 {
  font-size: 1.1em;
  margin: 0 0 15px 0;
  color: #333;
  font-weight: 600;
}

.summary-content {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  line-height: 1.6;
  color: #555;
  border-left: 4px solid #1a73e8;
}

.dialog-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}

.original-btn {
  background-color: #1a73e8;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9em;
  font-weight: 500;
  transition: background-color 0.2s;
}

.original-btn:hover {
  background-color: #1557b0;
}

/* 모바일 반응형 */
@media (max-width: 768px) {
  .dialog-content {
    width: 95%;
    max-height: 90vh;
    margin: 20px;
  }

  .dialog-title {
    font-size: 1.2em;
  }

  .post-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }

  .summary-content {
    padding: 12px;
  }
}
</style>
