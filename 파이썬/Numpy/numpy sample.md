import numpy as np
import matplotlib.pyplot as plt

# 1. 가상의 데이터 생성
np.random.seed(42)  # 재현성을 위한 시드 설정
data = np.random.normal(loc=50, scale=10, size=1000)  # 평균 50, 표준편차 10인 1000개의 데이터

# 2. 기초 통계 분석
mean = np.mean(data)  # 평균
median = np.median(data)  # 중앙값
std_dev = np.std(data)  # 표준편차
percentile_25 = np.percentile(data, 25)  # 25번째 백분위수
percentile_75 = np.percentile(data, 75)  # 75번째 백분위수

# 3. 분석 결과 출력
print(f"평균: {mean:.2f}")
print(f"중앙값: {median:.2f}")
print(f"표준편차: {std_dev:.2f}")
print(f"25번째 백분위수: {percentile_25:.2f}")
print(f"75번째 백분위수: {percentile_75:.2f}")

# 4. 데이터 시각화
plt.figure(figsize=(10, 6))

# 히스토그램
plt.hist(data, bins=30, color='skyblue', edgecolor='black', alpha=0.7, label='Data')

# 평균선 표시
plt.axvline(mean, color='red', linestyle='--', linewidth=2, label=f'Mean ({mean:.2f})')

# 중앙값 표시
plt.axvline(median, color='green', linestyle='-', linewidth=2, label=f'Median ({median:.2f})')

# 제목 및 레이블
plt.title('Data Distribution', fontsize=16)
plt.xlabel('Value', fontsize=14)
plt.ylabel('Frequency', fontsize=14)
plt.legend(fontsize=12)
plt.grid(True, alpha=0.3)

# 그래프 출력
plt.show()
