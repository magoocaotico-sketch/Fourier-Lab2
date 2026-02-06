/* ============================================
   CONFIGURACIÓN DEL SISTEMA
   ============================================ */
const CONFIG = {
    harmonics: 5,
    baseFreq: 200,
    colors: {
        grid: '#002200',
        inputSignal: '#00ff00',
        outputSignal: '#00aaff',
        magnitude: '#ffff00',
        phase: '#ff003c',
        bodeLine: '#666666'
    }
};

/* ============================================
   ESTADO GLOBAL
   ============================================ */
let state = {
    harmonics: Array.from({length: CONFIG.harmonics}, (_, i) => ({
        amp: i===0 ? 80 : 0,
        freq: (i+1) * CONFIG.baseFreq
    })),
    filter: { 
        fc: 1000,
        n: 2,
        type: 'butterworth'
    },
    animating: true,
    time: 0
};

/* ============================================
   GENERACIÓN DINÁMICA DE UI
   ============================================ */
const container = document.getElementById('harmonics-container');

state.harmonics.forEach((h, i) => {
    const div = document.createElement('div');
    div.className = 'harmonic-row';
    div.innerHTML = `
        <div class="row-header">
            <span style="font-size:0.75rem; color:var(--text-main)">H${i+1}</span>
        </div>
        
        <div class="slider-group">
            <label>Amp</label>
            <input type="range" min="0" max="100" value="${h.amp}" data-idx="${i}" data-type="amp">
            <div class="digital-display" id="disp-amp-${i}">${h.amp}</div>
        </div>
        
        <div class="slider-group">
            <label>Hz</label>
            <input type="range" min="50" max="5000" step="10" value="${h.freq}" data-idx="${i}" data-type="freq">
            <div class="digital-display" id="disp-freq-${i}">${h.freq}</div>
        </div>
    `;
    container.appendChild(div);
});

/* ============================================
   EVENT LISTENERS - ARMÓNICOS
   ============================================ */
container.addEventListener('input', (e) => {
    if(e.target.tagName === 'INPUT') {
        const idx = e.target.dataset.idx;
        const type = e.target.dataset.type;
        const val = parseFloat(e.target.value);
        
        state.harmonics[idx][type] = val;
        document.getElementById(`disp-${type}-${idx}`).innerText = val;
        
        if(!state.animating) renderTimeDomain();
        if(type === 'freq') renderBode();
    }
});

/* ============================================
   EVENT LISTENERS - FILTRO
   ============================================ */
const elFc = document.getElementById('cutoff');
const elOrd = document.getElementById('order');
const elType = document.getElementById('filterType');

function updateFilterState() {
    state.filter.fc = parseFloat(elFc.value);
    state.filter.n = parseInt(elOrd.value);
    state.filter.type = elType.value;
    
    document.getElementById('disp-fc').innerText = state.filter.fc + " Hz";
    document.getElementById('disp-order').innerText = state.filter.n;
    
    renderBode();
    if(!state.animating) renderTimeDomain();
}

elFc.addEventListener('input', updateFilterState);
elOrd.addEventListener('input', updateFilterState);
elType.addEventListener('change', updateFilterState);

/* ============================================
   CONTROL DE ANIMACIÓN
   ============================================ */
document.getElementById('animBtn').addEventListener('click', () => {
    state.animating = !state.animating;
    if(state.animating) animate();
});

/* ============================================
   PRESETS DE FORMAS DE ONDA
   ============================================ */
function setPreset(type) {
    state.harmonics.forEach((h, i) => {
        h.amp = 0;
        h.freq = (i+1) * CONFIG.baseFreq; 
        document.querySelector(`input[data-idx="${i}"][data-type="freq"]`).value = h.freq;
        document.getElementById(`disp-freq-${i}`).innerText = h.freq;
    });

    if(type === 'sine') state.harmonics[0].amp = 80;
    if(type === 'square') {
        state.harmonics.forEach((h,i) => { 
            if((i+1)%2!==0) h.amp = 80/(i+1); 
        });
    }
    if(type === 'triangle') {
        state.harmonics.forEach((h,i) => { 
            if((i+1)%2!==0) h.amp = 80/Math.pow(i+1, 2); 
        });
    }
    if(type === 'saw') {
        state.harmonics.forEach((h,i) => h.amp = 80/(i+1));
    }

    state.harmonics.forEach((h, i) => {
        document.querySelector(`input[data-idx="${i}"][data-type="amp"]`).value = h.amp;
        document.getElementById(`disp-amp-${i}`).innerText = h.amp.toFixed(1);
    });
    
    renderBode();
    if(!state.animating) renderTimeDomain();
}

/* ============================================
   FUNCIÓN DE RESET
   ============================================ */
function resetAll() {
    setPreset('sine');
    elFc.value = 1000; 
    elOrd.value = 2; 
    elType.value = 'butterworth';
    updateFilterState();
}

/* ============================================
   CÁLCULO DE FUNCIÓN DE TRANSFERENCIA
   ============================================ */
function calculateTransferFunction(f) {
    const { fc, n, type } = state.filter;
    const s = 2 * Math.PI * f;
    const wc = 2 * Math.PI * fc;
    
    let numerator = { re: 1, im: 0 };
    let denominator = { re: 1, im: 0 };

    if (type === 'butterworth') {
        numerator.re = Math.pow(wc, n);
        
        for (let k = 1; k <= n; k++) {
            const angle = Math.PI * (0.5 + (2 * k - 1) / (2 * n));
            const poleRe = wc * Math.cos(angle);
            const poleIm = wc * Math.sin(angle);
            
            const factorRe = -poleRe;
            const factorIm = s - poleIm;
            
            const newRe = denominator.re * factorRe - denominator.im * factorIm;
            const newIm = denominator.re * factorIm + denominator.im * factorRe;
            denominator.re = newRe;
            denominator.im = newIm;
        }
    } else {
        const ripple_dB = 1;
        const eps = Math.sqrt(Math.pow(10, ripple_dB / 10) - 1);
        const beta = Math.asinh(1 / eps) / n;
        
        for (let k = 1; k <= n; k++) {
            const theta = Math.PI * (2 * k - 1) / (2 * n);
            const poleRe = -wc * Math.sinh(beta) * Math.sin(theta);
            const poleIm = wc * Math.cosh(beta) * Math.cos(theta);
            
            const factorRe = -poleRe;
            const factorIm = s - poleIm;
            
            const newRe = denominator.re * factorRe - denominator.im * factorIm;
            const newIm = denominator.re * factorIm + denominator.im * factorRe;
            denominator.re = newRe;
            denominator.im = newIm;
        }
        
        if (n % 2 === 0) {
            numerator.re = Math.abs(denominator.re) / Math.sqrt(1 + eps * eps);
        } else {
            numerator.re = Math.abs(denominator.re);
        }
    }

    const denomMagSq = denominator.re * denominator.re + denominator.im * denominator.im;
    
    const Hre = (numerator.re * denominator.re + numerator.im * denominator.im) / denomMagSq;
    const Him = (numerator.im * denominator.re - numerator.re * denominator.im) / denomMagSq;
    
    const mag = Math.sqrt(Hre * Hre + Him * Him);
    const phase = Math.atan2(Him, Hre);

    return { mag: mag, phase: phase };
}

/* ============================================
   REFERENCIAS A CANVAS
   ============================================ */
const tCanvas = document.getElementById('timeCanvas');
const mCanvas = document.getElementById('magCanvas');
const pCanvas = document.getElementById('phaseCanvas');
const tCtx = tCanvas.getContext('2d');
const mCtx = mCanvas.getContext('2d');
const pCtx = pCanvas.getContext('2d');

/* ============================================
   REDIMENSIONAMIENTO DE CANVAS
   ============================================ */
function resizeCanvases() {
    [tCanvas, mCanvas, pCanvas].forEach(c => {
        c.width = c.parentElement.clientWidth;
        c.height = c.parentElement.clientHeight;
    });
    renderBode();
    if(!state.animating) renderTimeDomain();
}

new ResizeObserver(resizeCanvases).observe(document.querySelector('main'));

/* ============================================
   DIBUJAR REJILLA
   ============================================ */
function drawGrid(ctx, w, h, type = 'linear') {
    ctx.strokeStyle = CONFIG.colors.grid;
    ctx.lineWidth = 1;
    ctx.beginPath();
    
    if(type === 'log') {
        const minLog = Math.log10(50);
        const maxLog = Math.log10(10000);
        const scale = w / (maxLog - minLog);
        
        for(let i=1; i<10; i++) {
            [10, 100, 1000].forEach(base => {
                let freq = base * i;
                if(freq >= 50 && freq <= 10000) {
                    let x = (Math.log10(freq) - minLog) * scale;
                    ctx.moveTo(x, 0); 
                    ctx.lineTo(x, h);
                }
            });
        }
    } else {
        for(let x=0; x<w; x+=w/10) { 
            ctx.moveTo(x,0); 
            ctx.lineTo(x,h); 
        }
    }
    
    for(let y=0; y<h; y+=h/4) { 
        ctx.moveTo(0,y); 
        ctx.lineTo(w,y); 
    }
    ctx.stroke();
}

/* ============================================
   RENDERIZADO: DOMINIO DEL TIEMPO
   ============================================ */
function renderTimeDomain() {
    const w = tCanvas.width;
    const h = tCanvas.height;
    const cy = h / 2;
    
    tCtx.fillStyle = '#000'; 
    tCtx.fillRect(0,0,w,h);
    drawGrid(tCtx, w, h);

    tCtx.lineWidth = 2;
    let clipping = false;
    
    tCtx.beginPath();
    for(let x=0; x<w; x+=2) {
        let t = (x/w)*0.02 + state.time;
        let y = 0;
        
        state.harmonics.forEach(s => {
            if(s.amp > 0) y += s.amp * Math.sin(2 * Math.PI * s.freq * t);
        });
        
        let py = cy - y * (h/250);
        x===0 ? tCtx.moveTo(x, py) : tCtx.lineTo(x, py);
    }
    tCtx.strokeStyle = CONFIG.colors.inputSignal;
    tCtx.shadowBlur = 5;
    tCtx.shadowColor = CONFIG.colors.inputSignal;
    tCtx.stroke();
    tCtx.shadowBlur = 0;

    tCtx.beginPath();
    for(let x=0; x<w; x+=2) {
        let t = (x/w)*0.02 + state.time;
        let y = 0;
        
        state.harmonics.forEach(s => {
            if(s.amp > 0) {
                const tf = calculateTransferFunction(s.freq);
                y += (s.amp * tf.mag) * Math.sin(2 * Math.PI * s.freq * t + tf.phase);
            }
        });
        
        if(Math.abs(y) > 100) clipping = true;
        
        let py = cy - y * (h/250);
        x===0 ? tCtx.moveTo(x, py) : tCtx.lineTo(x, py);
    }
    tCtx.strokeStyle = CONFIG.colors.outputSignal;
    tCtx.shadowBlur = 10;
    tCtx.shadowColor = CONFIG.colors.outputSignal;
    tCtx.stroke();
    tCtx.shadowBlur = 0;

    const clipEl = document.getElementById('clip-indicator');
    if(clipping) clipEl.classList.add('clipping');
    else clipEl.classList.remove('clipping');
}

/* ============================================
   RENDERIZADO: DIAGRAMAS DE BODE
   ============================================ */
function renderBode() {
    const mw = mCanvas.width; 
    const mh = mCanvas.height;
    mCtx.clearRect(0,0,mw,mh);
    drawGrid(mCtx, mw, mh, 'log');

    const minLog = Math.log10(50);
    const maxLog = Math.log10(10000);
    const scaleX = mw / (maxLog - minLog);

    mCtx.beginPath();
    mCtx.strokeStyle = CONFIG.colors.bodeLine;
    mCtx.lineWidth = 2;
    mCtx.setLineDash([5, 5]);
    
    for(let x=0; x<mw; x+=2) {
        const f = Math.pow(10, (x/scaleX) + minLog);
        const tf = calculateTransferFunction(f);
        const db = 20 * Math.log10(Math.max(tf.mag, 1e-10));
        const y = mh * 0.5 - (db * (mh * 0.02));
        x===0 ? mCtx.moveTo(x,y) : mCtx.lineTo(x,y);
    }
    mCtx.stroke();
    mCtx.setLineDash([]);

    mCtx.fillStyle = CONFIG.colors.magnitude;
    state.harmonics.forEach((h, idx) => {
        if(h.freq >= 50 && h.freq <= 10000) {
            const tf = calculateTransferFunction(h.freq);
            const magOut = h.amp * tf.mag;
            const x = (Math.log10(h.freq) - minLog) * scaleX;
            const barHeight = (magOut / 100) * (mh * 0.6);
            
            if(barHeight > 0.5 || h.amp > 0) {
                mCtx.fillRect(x-3, mh - Math.max(barHeight, 2), 6, Math.max(barHeight, 2));
            }
        }
    });

    const pw = pCanvas.width; 
    const ph = pCanvas.height;
    pCtx.clearRect(0,0,pw,ph);
    drawGrid(pCtx, pw, ph, 'log');
    const scalePX = pw / (maxLog - minLog);

    pCtx.beginPath();
    pCtx.strokeStyle = '#555';
    pCtx.lineWidth = 2;
    for(let x=0; x<pw; x+=2) {
        const f = Math.pow(10, (x/scalePX) + minLog);
        const tf = calculateTransferFunction(f);
        const deg = tf.phase * (180/Math.PI);
        const y = ph * 0.5 - (deg * (ph/180));
        x===0 ? pCtx.moveTo(x,y) : pCtx.lineTo(x,y);
    }
    pCtx.stroke();

    pCtx.fillStyle = CONFIG.colors.phase;
    state.harmonics.forEach((h, idx) => {
        if(h.freq >= 50 && h.freq <= 10000) {
            const tf = calculateTransferFunction(h.freq);
            const x = (Math.log10(h.freq) - minLog) * scalePX;
            const deg = tf.phase * (180/Math.PI);
            const y = ph * 0.5 - (deg * (ph/180));
            
            pCtx.beginPath(); 
            pCtx.arc(x, y, h.amp > 0 ? 5 : 3, 0, Math.PI*2); 
            pCtx.fill();
        }
    });
}

/* ============================================
   LOOP DE ANIMACIÓN
   ============================================ */
function animate() {
    if(!state.animating) return;
    state.time += 0.0003;
    renderTimeDomain();
    requestAnimationFrame(animate);
}

animate();