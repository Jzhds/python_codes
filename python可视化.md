### 可视化

```python
import matplotlib.pyplot as plt
x = [i for i in range(1,11)]
y = [i**2 for i in range(1,11)]
plt.title("jkj")
plt.xlabel('Value', fontsize=18)
plt.ylabel('Square', fontsize=18)
    # 设置刻度标记的文字大小
plt.tick_params(axis='both', labelsize=16)
    # 绘制折线图
plt.plot(x, y) #把plot改成scatter即为画散点图
plt.axis([0, 12, 0, 120]) #规定坐标轴范围
plt.show()

#绘制正弦曲线
# 指定采样的范围以及样本的数量
x_values = np.linspace(0, 2 * np.pi, 1000)
# 计算每个样本对应的正弦值
y_values = np.sin(x_values)
# 绘制折线图(线条形状为--, 颜色为蓝色)
plt.plot(x_values, y_values, '--b')
plt.plot(x_values, np.sin(2 * x_values), '--r')
plt.show()

# 绘制两条曲线
# 将样本数量减少为50个
x_values = np.linspace(0, 2 * np.pi, 50)
# 设置绘图为2行1列活跃区为1区(第一个图)
plt.subplot(2, 1, 1)
plt.plot(x_values, np.sin(x_values), 'o-b')
# 设置绘图为2行1列活跃区为2区(第二个图)
plt.subplot(2, 1, 2)
plt.plot(x_values, np.sin(2 * x_values), '.-r')
plt.show()

#绘制正态分布的直方图
data = np.random.normal(10.0, 5.0, 1000)
# 绘制直方图(直方的数量为10个)
plt.hist(data, 10)
plt.show()
```

