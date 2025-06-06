## 1. Постановка задачи

Рассматривается дифференциальное уравнение третьего порядка с краевыми условиями:

$$\begin{cases}
    u^{(3)}+8u=f(x) \\
    u(0)=u'(0)=0,\quad \\
    u(1)=\alpha
\end{cases}$$


Необходимо построить разностную схему второго порядка аппроксимации и решить уравнение при различных значениях
$$h$$ (шаг сетки) и $$\alpha$$.


## 2. Метод решения
Для получения разностной аппроксимации производных второго и третьего порядков в задаче с уравнением 

$$ u^{(3)}(x) + 8u(x) = f(x) $$

 с шагом сетки $$ h $$, используются стандартные схемы конечных разностей. Ниже представлены аппроксимация для первой, второй и третьей производных с **вторым порядком точности**.

#### 1. Разностная аппроксимация первой производной $$ u'(x) $$

Используем центральную разностную аппроксимацию для первой производной $$ u'(x) $$:  

$$
u'(x_i) \approx \frac{u_{i+1} - u_{i}}{h} + O(h^2).
$$

Где $$ u_i = u(x_i) $$ — значение функции в узле $$ x_i = i \cdot h $$, а $$ h $$ — шаг сетки.

#### 2. Разностная аппроксимация второй производной $$ u''(x) $$

Вторая производная $$ u''(x) $$ аппроксимируется центральной разностной схемой:

$$
u''(x_i) \approx \frac{u_{i+1} - 2u_i + u_{i-1}}{h^2} + O(h^2).
$$

Эта схема также обладает вторым порядком точности.

#### 3. Разностная аппроксимация третьей производной $$ u^{(3)}(x) $$

Для третьей производной $$ u^{(3)}(x) $$ применим центральную разностную схему второго порядка:

$$
u^{(3)}(x_i) \approx \frac{u_{i+2} - 2u_{i+1} + 2u_{i-1} - u_{i-2}}{2h^3} + O(h^2).
$$

Эта схема аппроксимирует третью производную с **вторым порядком точности**.

#### 4. Подстановка в уравнение

Подставим эти аппроксимации в исходное уравнение 

$$ u^{(3)}(x) + 8u(x) = f(x) $$ 

в узле $$x_i $$:

$$
\frac{u_{i+2} - 2u_{i+1} + 2u_{i-1} - u_{i-2}}{2h^3} + 8u_i = f_i.
$$

Где $$ f_i = f(x_i) $$.

#### 5. Граничные условия

Граничные условия $$ u(0) = u'(0) = 0 $$ и $$ u(1) = \alpha $$ задаются явно:
- $$ u_0 = 0 $$,

- $$ \frac{u_1 - u_{0}}{h} = 0  $$ 
(приближенное граничное условие для первой производной в точке $$ x = 0 $$),

- $$ u_{N+1} = \alpha $$, где $$ N+1 $$ — индекс последнего узла сетки (соответствующего $$ x = 1 $$).

#### 6. Составление системы линейных алгебраических уравнений (СЛАУ)

После дискретизации дифференциального уравнения и применения граничных условий, получаем систему линейных уравнений на значения функции $ u_i $ в узлах сетки. Эта система записывается в виде матричного уравнения:
$$
A \mathbf{u} = \mathbf{f},
$$
где:
- $$ A $$ — матрица, состоящая из коэффициентов разностной аппроксимации,
- $$ \mathbf{u} $$ — вектор значений функции $$ u_i $$,
- $$ \mathbf{f} $$ — вектор значений правой части $$ f_i $$.

Неизвестными у нас будут значения $$u$$ во внутренних узлах интерполяции: $$u_1,...,u_N$$

Из условий $$u'(0) = 0$$ и $$u(0)=0$$ получим: 

$$0=\frac{u_{1} - u_{0}}{h}$$

откуда  

$$u_1=0$$

Для вычисления третьей производной в точке $$x_1$$ нам понадобится ввести вспомогательную переменную $$u_{-1}$$. Воспользуемся приближением:

 $$u_{-1} = u(-\frac{1}{h})≈ u(0)-u'(0)h=0$$

Получим систему из $$N-1$$ линейных уравнений для оставшихся $$N-1$$ неизвестных $$u_2,...,u_N$$

$$\begin{cases}
  -\frac{1}{h^3}u_{2}+\frac{1}{2h^3}u_{3} = f_1 \\
  8u_2-\frac{1}{h^3}u_{3}+\frac{1}{2h^3}u_{4} = f_2 \\
  -\frac{1}{2h^3}u_{i-2} + \frac{1}{h^3}u_{i-1}+8u_i-\frac{1}{h^3}u_{i+1}+\frac{1}{2h^3}u_{i+2} = f_i &\text{,   $i=3,...,N-2$} \\
  -\frac{1}{2h^3}u_{N-3} + \frac{1}{h^3}u_{N-2}+8u_{N-1}-\frac{1}{h^3}u_{N}+\frac{1}{2h^3}\alpha = f_{N-1}
\end{cases}$$

#### Итог:

Получив систему уравнений, её можно решить численно, используя стандартные методы решения систем линейных уравнений

## 3. Корректность
Для проверки корректности будем использовать следующие функции в правой части:
- $$f_1(x) \equiv 0$$
- $$f_2(x)=e^x$$
- $$f_3(x)=xe^x$$
- $$f_4(x)=\sin{x}+\cos{x}$$

Общее решение однородного уравнения (при правой части $$f_1(x) \equiv 0$$) имеет вид:

 $$u_{oo}(x)=C_1e^{-2x}+e^{x}(C_2\cos{\sqrt{3}x}+C_2\sin{\sqrt{3}x}), \quad C_i\in\mathbb{R}, \quad i=1,2,3$$

Общее решение неоднородного уравнения при правой части $$f_2(x)=e^x$$ имеет вид:

 $$u_{oo}(x)=C_1e^{-2x}+e^{x}(C_2\cos{\sqrt{3}x}+C_2\sin{\sqrt{3}x}) + \frac{1}{9}e^x, \quad C_i\in\mathbb{R}, \quad i=1,2,3$$

При правой части $$f_3(x)=xe^x$$ :

$$u_{oo}(x)=C_1e^{-2x}+e^{x}(C_2\cos{\sqrt{3}x}+C_2\sin{\sqrt{3}x}) + \frac{1}{9}(x-\frac{1}{3})e^x, \quad C_i\in\mathbb{R}, \quad i=1,2,3$$

При правой части $$f_4(x)=\sin{x}+\cos{x}$$:

$$u_{oo}(x)=C_1e^{-2x}+e^{x}(C_2\cos{\sqrt{3}x}+C_2\sin{\sqrt{3}x}) + \frac{1}{9}(x-\frac{1}{3})e^x, \quad C_i\in\mathbb{R}, \quad i=1,2,3$$



