<template>
    <div class="container">
        <div class="row" v-if="showIntro">
            <div class="col col-12 mb-4 mt-4">
                <h1 class="mb-4">Моделирование простой гидравлической системы</h1>

                <p>
                    В химическом производстве работает множество аппаратов и происходит большое количество процессов.
                    Но прежде чем тратить деньги на строительство завода, делают его математическую модель. Такая модель
                    позволяет подобрать нужные параметры различных аппаратов и избежать дорогостоящих экспериментов.
                </p>
                <p>
                    Математическая модель производства сложна. Полноценные модели считаются при помощи специальных
                    программ. Разобраться с тем, как работают подобные программы намного легче, понимая основы.
                </p>
                <p>
                    Гидравлическая система служит для доставки и хранения жидких реагентов. Она является одной из самых
                    простых подсистем химического производства, на примере которой удобно разобраться с основными способами,
                    как составления и расчета реальных математических моделей.
                </p>
                <p>
                    Чтобы начать работу над моделью, сформулируем ее в виде задачи.
                </p>

                <button class="btn btn-success btn-lg btn-block" @click="showIntro = false">Приступить к задаче</button>
            </div>
        </div>
        <div class="row" v-else-if="showTask">
            <div class="col col-12 mb-4 mt-4">
                <h1 class="mb-4">Задача</h1>

                <p>
                    На схеме представлена гидравлическая система, состоящая из двух стоящих рядом емкостей, соединяющего их
                    трубопровода и вентилей для регулировки потока жидкости
                </p>

                <p class="text-center">
                    <img src="./assets/var4.png">
                </p>

                <p>
                    Известны следующие параметры:
                </p>
                <p>
                    Давления жидкости на входе в систему (слева на схеме), Па:
                    P<sub>1</sub> = 7.82 ⋅ 10<sup>6</sup>,
                    P<sub>2</sub> = 9.41 ⋅ 10<sup>6</sup>,
                    P<sub>3</sub> = 7.45 ⋅ 10<sup>5</sup>,
                    P<sub>4</sub> = 2.55 ⋅ 10<sup>5</sup>,
                    P<sub>5</sub> = 8.26 ⋅ 10<sup>5</sup>,
                    P<sub>6</sub> = 2.26 ⋅ 10<sup>5</sup>,
                </p>
                <p>
                    Коэффициенты пропускной способности вентилей:
                    K<sub>1</sub> = 0.01,
                    K<sub>2</sub> = 0.01,
                    K<sub>3</sub> = 0.01,
                    K<sub>4</sub> = 0.01,
                    K<sub>5</sub> = 0.01,
                    K<sub>6</sub> = 0.01,
                    K<sub>7</sub> = 0.002,
                </p>
                <p>
                    Высоты ёмкостей, м:
                    H<sup>G</sup><sub>1</sub> = 9,
                    H<sup>G</sup><sub>2</sub> = 8
                </p>
                <p>
                    Прочие параметры:
                    Плотность жидкости, ρ<sub>ж</sub> = 1000 кг/м<sup>3</sup>,
                    Начальное давление воздуха в ёмкости, P<sub>н</sub> = 10<sup>5</sup> Па,
                    Площадь сечения трубопровода, S<sub>труб</sub> = 0.01 м<sup>2</sup>
                </p>

                <p>
                    Для этой гидравлической системы:
                </p>
                <ol>
                    <li>Построить математическое описание процесса движения жидкости в стационарном режиме;</li>
                    <li>Построить информационную матрицу системы уравнений для выбора декомпозиционного алгоритма решения;</li>
                    <li>Построить блок-схему алгоритма решения прямой задачи, включающей стандартные численные методы вычислительной математики;</li>
                    <li>Вычислить неизвестные велчины: P<sub>7</sub> &mdash; P<sub>10</sub>, V<sub>1</sub> &mdash; V<sub>7</sub>, h<sub>1</sub>, h<sub>2</sub>;</li>
                    <li>Построить график зависимости h<sub>1</sub>(P<sub>1</sub>)</li>
                </ol>

                <h2>Решить эту задачу поможет обучающее видео:</h2>

                <p class="text-center">
                    <iframe
                            width="560"
                            height="315"
                            src="https://www.youtube.com/embed/videoseries?list=PLZ8wH2h6UeZKeEHKGfVlwR0r5opDRXi0k"
                            frameborder="0"
                            allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                            allowfullscreen
                    ></iframe>
                </p>

                <p>После того как уравнения будут готовы и расположены в правильном порядке, можно приступить к работе с тренажером</p>

                <button class="btn btn-success btn-lg btn-block" @click="showTask = false">Начать работу с тренажером</button>
            </div>
        </div>

        <Scheme :hG="hG" :h="h" :h0="[0,0]" :v="v" :p="p" v-if="!showIntro && !showTask"></Scheme>
        <div class="row" v-if="!showIntro && !showTask">
            <div class="col col-12">
                <div class="alert alert-danger" v-if="calculateError">{{calculateError}}</div>

                <div class="form-check">
                    <input class="form-check-input" type="checkbox" v-model="useAutoCalculate">
                    <label class="form-check-label">
                        Использовать авторасчет
                    </label>
                </div>
                <button class="btn btn-success btn-lg btn-block mb-4" @click="calculate">Расчитать</button>
            </div>
        </div>
        <form v-if="!showIntro && !showTask">
            <div class="row">
                <div class="col col-12 col-md-2">
                    <div class="form-group" v-for="index in 2" :key="index">
                        <label>H<sup>G</sup><sub>{{index}}</sub>={{hG[index-1]}} м</label>
                        <input type="range" class="form-control-range" min="1" max="10" step="0.1" v-model.number="hG[index-1]" @input="autoCalculate">
                    </div>
                </div>
                <div class="col col-12 col-md-2">
                    <div class="form-group" v-for="index in 6" :key="index">
                        <label>P<sub>{{index}}</sub>={{p[index-1]}} Па</label>
                        <input type="range" class="form-control-range" min="10000" max="10000000" step="10000" v-model.number="p[index-1]" @input="autoCalculate">
                    </div>
                </div>
                <div class="col col-12 col-md-2">
                    <div class="form-group" v-for="index in 7" :key="index">
                        <label>a<sub>{{index}}</sub>={{a[index-1]}}</label>
                        <input type="range" class="form-control-range" min="0" max="0.1" step="0.001" v-model.number="a[index-1]" @input="autoCalculate">
                    </div>
                </div>
                <div class="col col-12 col-md-6">
                    <div class="form-group">
                        <label>Уравнения</label>
                        <textarea class="form-control" rows="14" v-model="equationsText" @change="autoCalculate"></textarea>
                    </div>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
    import Scheme from "./components/Scheme";

    export default {
        name: 'App',
        components: {
            Scheme
        },
        data() {
            return {
                hG: [9, 8],
                p: [7.82e6, 9.41e6, 7.45e5, 2.55e5, 8.26e5, 2.26e5, 0, 0, 0, 0],
                patm: 101325,
                ro: 1000,
                g: 9.81,
                S: 0.01,

                h: [0, 0],
                a: [0.01, 0.01, 0.01, 0.01, 0.01, 0.01, 0.002],
                v: [0, 0, 0, 0, 0, 0, 0],

                showIntro: true,
                showTask: true,
                autoCalculateInProcess: false,
                useAutoCalculate: true,
                calculateError: false,
                equationsText: `p[6] = patm * ( hG[0]/(hG[0] - h[0]) );
p[8] = p[6] + ro * g * h[0];
v[0] = k[0] * sign( p[0] - p[8] ) * sqrt( abs(p[0] - p[8]) );
v[2] = k[2] * sign( p[8] - p[2] ) * sqrt( abs(p[8] - p[2]) );
v[3] = k[3] * sign( p[8] - p[3] ) * sqrt( abs(p[8] - p[3]) );
v[6] = v[0] - v[2] - v[3];
p[9] = p[8] - sign(v[6]) * pow(v[6], 2)/pow(k[6], 2);
v[1] = k[1] * sign( p[1] - p[9] ) * sqrt( abs(p[1] - p[9]) );
v[4] = k[4] * sign( p[9] - p[4] ) * sqrt( abs(p[9] - p[4]) );
v[5] = k[5] * sign( p[9] - p[5] ) * sqrt( abs(p[9] - p[5]) );`,
            }
        },
        methods: {

            clone(srcArray) {
                return srcArray.slice();
            },

            calculateVars(h1guess) {
                let p = this.clone(this.p);
                let patm = this.patm;
                let hG = this.clone(this.hG);
                let ro = this.ro;
                let g = this.g;
                let k = this.clone(this.k);
                let v = this.clone(this.v);
                let h = [h1guess, 0];

                let sqrt = Math.sqrt;
                let abs = Math.abs;
                let pow = Math.pow;
                let sign = Math.sign;

                try {
                    eval(this.equationsText);
                }
                catch (e) {
                    this.calculateError = false;
                }

                return {p, v};
            },
            calculateBalance(h1guess) {
                let {v} = this.calculateVars(h1guess);
                return v[1] + v[6] - v[4] - v[5];
            },
            solveSqEq(a, b, c) {
                let result = (-b + Math.sqrt( b*b - 4 * a * c ))/(2 * a)
                if (result < 0) {
                    result = (-b - Math.sqrt( b*b - 4 * a * c ))/(2 * a);
                }

                return result;
            },
            calculate() {
                this.calculateError = false;

                let precision = 0.01;
                let a = precision;
                let b = this.hG[0] - precision;
                let h = (a+b)/2;
                let intervalSize = Math.abs(a-b);

                let f_a = this.calculateBalance(a);
                let f_b = this.calculateBalance(b);
                let f_h = this.calculateBalance(h);
                let noSignChange = f_a * f_b > 0;


                if (noSignChange) {
                    this.calculateError = 'Квадратное уравнение не имеет корней';
                    return false;
                }

                while (intervalSize > precision) {
                    let leftIntervalSignChange = f_a * f_h < 0;

                    if (leftIntervalSignChange) {
                        b = h;
                        f_b = f_h;
                    }
                    else {
                        a = h;
                        f_a = f_h;

                    }

                    h = (a+b)/2;
                    f_h = this.calculateBalance(h);
                    intervalSize = Math.abs(a-b);
                }

                let {p, v} = this.calculateVars(h);

                let h2 = this.solveSqEq(
                    -this.ro * this.g,
                    (p[9]+this.ro * this.g * this.hG[1]),
                    (this.patm-p[9])*this.hG[1]
                );

                p[7] = this.patm * this.hG[1] / (this.hG[1] - h2);

                if (h2 < 0 || h2 >= this.hG[2]) {
                    this.calculateError = 'H2 выходит из области допустимых значений';
                    return false;
                }

                this.p = p;
                this.v = v;
                this.h = [h, h2];


                return true;
            },
            autoCalculate() {
                if (this.useAutoCalculate) {
                    this.calculate();
                }
            }

        },
        computed: {
            k() {
                return this.a.map( a => a * this.S / Math.sqrt(this.ro) );
            }
        }
    }
</script>

<style scoped>
    @media (max-width: 768px) {
        iframe, img {
            width: 100%!important;
            height: auto;
        }
    }
</style>
