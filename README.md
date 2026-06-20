# Fintech-
import React, { useState, useEffect, useMemo, useRef } from 'react';
import { 
  Shield, Activity, Globe, Cpu, Database, TrendingUp, TrendingDown, 
  Wallet, Upload, FileText, CheckCircle, AlertTriangle, RefreshCw, Info, Download, Filter 
} from 'lucide-react';

// Brand Identity Palette
const BKASH_PINK = '#E2125B';
const DARK_BG = '#0B0F19';
const MAP_GREEN = '#006A4E';

// Comprehensive District-Level Database of Bangladesh (64 Districts mapped under 8 Divisions)
const DISTRICTS_DATA = {
  'Dhaka Division': {
    'Dhaka City': { cashIn: 4500000, cashOut: 4800000, status: 'Normal', latitude: 230, longitude: 200 },
    'Gazipur': { cashIn: 2800000, cashOut: 2100000, status: 'Good', latitude: 210, longitude: 205 },
    'Narayanganj': { cashIn: 3200000, cashOut: 3900000, status: 'Warning', latitude: 245, longitude: 215 },
    'Tangail': { cashIn: 1800000, cashOut: 2400000, status: 'Critical', latitude: 195, longitude: 180 },
    'Faridpur': { cashIn: 1400000, cashOut: 1350000, status: 'Good', latitude: 250, longitude: 170 },
    'Munshiganj': { cashIn: 1600000, cashOut: 1900000, status: 'Normal', latitude: 260, longitude: 210 },
    'Manikganj': { cashIn: 1100000, cashOut: 980000, status: 'Good', latitude: 228, longitude: 175 },
    'Narsingdi': { cashIn: 1950000, cashOut: 1800000, status: 'Good', latitude: 215, longitude: 228 },
    'Gopalganj': { cashIn: 1250000, cashOut: 1300000, status: 'Normal', latitude: 285, longitude: 175 },
    'Madaripur': { cashIn: 1350000, cashOut: 1700000, status: 'Warning', latitude: 275, longitude: 190 },
    'Shariatpur': { cashIn: 1150000, cashOut: 1200000, status: 'Normal', latitude: 280, longitude: 205 },
    'Rajbari': { cashIn: 950000, cashOut: 1100000, status: 'Normal', latitude: 240, longitude: 155 },
    'Kishoreganj': { cashIn: 1700000, cashOut: 2150000, status: 'Warning', latitude: 185, longitude: 235 }
  },
  'Chittagong Division': {
    'Chittagong City': { cashIn: 3900000, cashOut: 3500000, status: 'Good', latitude: 350, longitude: 330 },
    'Cox\'s Bazar': { cashIn: 2100000, cashOut: 2900000, status: 'Critical', latitude: 410, longitude: 355 },
    'Comilla': { cashIn: 2700000, cashOut: 2300000, status: 'Good', latitude: 280, longitude: 265 },
    'Feni': { cashIn: 1400000, cashOut: 1100000, status: 'Good', latitude: 310, longitude: 290 },
    'Noakhali': { cashIn: 1850000, cashOut: 2200000, status: 'Warning', latitude: 330, longitude: 275 },
    'Chandpur': { cashIn: 1550000, cashOut: 1400000, status: 'Good', latitude: 300, longitude: 245 },
    'Brahmanbaria': { cashIn: 1900000, cashOut: 2100000, status: 'Normal', latitude: 248, longitude: 255 },
    'Rangamati': { cashIn: 850000, cashOut: 1150000, status: 'Warning', latitude: 315, longitude: 365 },
    'Khagrachhari': { cashIn: 720000, cashOut: 900000, status: 'Normal', latitude: 285, longitude: 345 },
    'Bandarban': { cashIn: 610000, cashOut: 950000, status: 'Critical', latitude: 375, longitude: 370 },
    'Lakshmipur': { cashIn: 1200000, cashOut: 1150000, status: 'Good', latitude: 335, longitude: 255 }
  },
  'Sylhet Division': {
    'Sylhet City': { cashIn: 2500000, cashOut: 1800000, status: 'Good', latitude: 160, longitude: 320 },
    'Moulvibazar': { cashIn: 1450000, cashOut: 1200000, status: 'Good', latitude: 195, longitude: 315 },
    'Habiganj': { cashIn: 1300000, cashOut: 1750000, status: 'Warning', latitude: 210, longitude: 285 },
    'Sunamganj': { cashIn: 1100000, cashOut: 1500000, status: 'Warning', latitude: 150, longitude: 280 }
  },
  'Rajshahi Division': {
    'Rajshahi City': { cashIn: 2200000, cashOut: 1900000, status: 'Good', latitude: 200, longitude: 90 },
    'Bogra': { cashIn: 2100000, cashOut: 2350000, status: 'Normal', latitude: 165, longitude: 125 },
    'Pabna': { cashIn: 1650000, cashOut: 1900000, status: 'Normal', latitude: 220, longitude: 115 },
    'Naogaon': { cashIn: 1400000, cashOut: 1300000, status: 'Good', latitude: 150, longitude: 95 },
    'Natore': { cashIn: 1150000, cashOut: 1350000, status: 'Normal', latitude: 190, longitude: 108 },
    'Joypurhat': { cashIn: 850000, cashOut: 920000, status: 'Good', latitude: 135, longitude: 110 },
    'Chapai Nawabganj': { cashIn: 1250000, cashOut: 1700000, status: 'Warning', latitude: 180, longitude: 60 },
    'Sirajganj': { cashIn: 1800000, cashOut: 2450000, status: 'Critical', latitude: 185, longitude: 145 }
  },
  'Khulna Division': {
    'Khulna City': { cashIn: 2100000, cashOut: 2450000, status: 'Warning', latitude: 335, longitude: 135 },
    'Jessore': { cashIn: 1900000, cashOut: 1600000, status: 'Good', latitude: 295, longitude: 120 },
    'Satkhira': { cashIn: 1300000, cashOut: 1850000, status: 'Critical', latitude: 350, longitude: 110 },
    'Bagerhat': { cashIn: 1150000, cashOut: 1100000, status: 'Good', latitude: 345, longitude: 155 },
    'Kushtia': { cashIn: 1450000, cashOut: 1300000, status: 'Good', latitude: 245, longitude: 110 },
    'Jhenaidah': { cashIn: 1100000, cashOut: 1250000, status: 'Normal', latitude: 270, longitude: 112 },
    'Magura': { cashIn: 850000, textOut: 900000, status: 'Good', latitude: 265, longitude: 135 },
    'Narail': { cashIn: 720000, cashOut: 850000, status: 'Normal', latitude: 290, longitude: 140 },
    'Chuadanga': { cashIn: 950000, cashOut: 1200000, status: 'Warning', latitude: 255, longitude: 90 },
    'Meherpur': { cashIn: 620000, cashOut: 780000, status: 'Normal', latitude: 240, longitude: 85 }
  },
  'Barishal Division': {
    'Barishal City': { cashIn: 1800000, cashOut: 1600000, status: 'Good', latitude: 330, longitude: 195 },
    'Bhola': { cashIn: 1200000, cashOut: 1750000, status: 'Critical', latitude: 350, longitude: 220 },
    'Patuakhali': { cashIn: 1350000, cashOut: 1500000, status: 'Normal', latitude: 380, longitude: 200 },
    'Pirojpur': { cashIn: 950000, cashOut: 1100000, status: 'Normal', latitude: 345, longitude: 175 },
    'Barguna': { cashIn: 850000, cashOut: 1150000, status: 'Warning', latitude: 390, longitude: 185 },
    'Jhalokati': { cashIn: 710000, cashOut: 820000, status: 'Good', latitude: 340, longitude: 185 }
  },
  'Rangpur Division': {
    'Rangpur City': { cashIn: 1950000, cashOut: 1600000, status: 'Good', latitude: 95, longitude: 115 },
    'Dinajpur': { cashIn: 1600000, cashOut: 1850000, status: 'Normal', latitude: 105, longitude: 85 },
    'Gaibandha': { cashIn: 1100000, cashOut: 1450000, status: 'Warning', latitude: 120, longitude: 135 },
    'Kurigram': { cashIn: 950000, cashOut: 1300000, status: 'Critical', latitude: 90, longitude: 145 },
    'Nilphamari': { cashIn: 1150000, cashOut: 980000, status: 'Good', latitude: 80, longitude: 100 },
    'Lalmonirhat': { cashIn: 780000, cashOut: 950000, status: 'Normal', latitude: 70, longitude: 125 },
    'Panchagarh': { cashIn: 900000, cashOut: 1100000, status: 'Normal', latitude: 45, longitude: 70 },
    'Thakurgaon': { cashIn: 1100000, cashOut: 1050000, status: 'Good', latitude: 75, longitude: 65 }
  },
  'Mymensingh Division': {
    'Mymensingh City': { cashIn: 2100000, cashOut: 1800000, status: 'Good', latitude: 155, longitude: 195 },
    'Jamalpur': { cashIn: 1350000, cashOut: 1600000, status: 'Warning', latitude: 145, longitude: 165 },
    'Netrokona': { cashIn: 1200000, cashOut: 1450000, status: 'Warning', latitude: 140, longitude: 220 },
    'Sherpur': { cashIn: 950000, cashOut: 1150000, status: 'Normal', latitude: 125, longitude: 180 }
  }
};

const INITIAL_TRANSACTIONS = [
  { id: 'BK-TX-9921', name: 'Zayan Kabir', amount: 145000, time: '05:10 AM', type: 'bKash SendMoney', score: 320, route: 'Suspense Account Hold', status: 'In Suspense Account', division: 'Dhaka Division', district: 'Dhaka City', reason: 'Simultaneous login session from suspicious biometric profile speed.', ip: '103.218.24.105', device: 'Rooted Emulator SDK-33', imsiVelocity: 'High (Sim swapped 2h ago)', location: 'Gulshan, Dhaka', networkTower: 'BTS-DHK-GUL2-09', timestamp: Date.now() - 60000 },
  { id: 'BK-TX-8843', name: 'Momena Begum', amount: 45000, time: '05:08 AM', type: 'Merchant Pay', score: 890, route: 'Normal Fast-Track', status: 'Settled', division: 'Chittagong Division', district: 'Chittagong City', reason: 'Low risk footprint matching historic GPS trajectory.', ip: '180.234.12.9', device: 'Xiaomi Note 10 Pro', imsiVelocity: 'Stable', location: 'Agrabad, Chittagong', networkTower: 'BTS-CTG-AGR4-12', timestamp: Date.now() - 120000 },
  { id: 'BK-TX-7731', name: 'Tanvir Rahman', amount: 120000, time: '05:06 AM', type: 'Agent Cash-Out', score: 480, route: 'Suspense Account Hold', status: 'Pending OTP Validation', division: 'Sylhet Division', district: 'Habiganj', reason: 'High proximity transaction near international border. Cross-matching SIM serial.', ip: '103.55.108.43', device: 'Samsung Galaxy S22', imsiVelocity: 'Moderate', location: 'Tamabil, Sylhet', networkTower: 'BTS-SYL-TAM1-02', timestamp: Date.now() - 180000 },
  { id: 'BK-TX-6612', name: 'Rahim Store', amount: 350000, time: '05:04 AM', type: 'Merchant Pay', score: 210, route: 'Hard Anti-Fraud Lock', status: 'Blocked & Flagged', division: 'Rajshahi Division', district: 'Sirajganj', reason: 'Velocity Limit Exceeded. Wallet farmed with micro-loans under 180s.', ip: '202.4.98.112', device: 'Unknown MTK Device', imsiVelocity: 'High-IMSI-Velocity', location: 'Motihar, Rajshahi', networkTower: 'BTS-RAJ-MOT3-44', timestamp: Date.now() - 240000 },
  { id: 'BK-TX-5509', name: 'Farhan Adil', amount: 8500, time: '05:02 AM', type: 'bKash SendMoney', score: 950, route: 'Normal Fast-Track', status: 'Settled', division: 'Khulna Division', district: 'Jessore', reason: 'Verified device hardware ID matching secure base-station footprint.', ip: '103.82.172.18', device: 'iPhone 13', imsiVelocity: 'Stable', location: 'Boyra, Khulna', networkTower: 'BTS-KLN-BOY1-05', timestamp: Date.now() - 300000 },
  { id: 'BK-TX-4491', name: 'Suhana Jasmine', amount: 28000, time: '05:00 AM', type: 'Agent Cash-Out', score: 550, route: 'Suspense Account Hold', status: 'Pending OTP Validation', division: 'Barishal Division', district: 'Bhola', reason: 'Unusual transaction interval from unregistered hardware configuration.', ip: '103.112.54.89', device: 'Infinix Hot 11', imsiVelocity: 'Stable', location: 'Sadar, Barishal', networkTower: 'BTS-BAR-SAD2-01', timestamp: Date.now() - 360000 }
];

const INITIAL_KPI_DATA = {
  'Dhaka Division': {
    avgDailyTarget: 4500000,
    historicalPerformance: [
      { day: 'Day -5', actual: 4420000, achieveRate: 98, primaryReason: 'Extended wholesale market operational hours.' },
      { day: 'Day -4', actual: 4610000, achieveRate: 102, primaryReason: 'High salary cash disbursements weekend.' },
      { day: 'Day -3', actual: 4850000, achieveRate: 107, primaryReason: 'Pre-holiday retail market commerce surge.' },
      { day: 'Day -2', actual: 4200000, achieveRate: 93, primaryReason: 'Severe regional weather causing transport blockades.' },
      { day: 'Day -1', actual: 4580000, achieveRate: 101, primaryReason: 'Corporate utility bill digital collection window.' }
    ],
    predictedDrivers: {
      increase: 'Inbound salary transfers & dynamic merchant pay campaigns.',
      decrease: 'Unexpected banking clearing network maintenance windows.'
    }
  },
  'Chittagong Division': {
    avgDailyTarget: 3800000,
    historicalPerformance: [
      { day: 'Day -5', actual: 3610000, achieveRate: 95, primaryReason: 'Port labor settlement cycles.' },
      { day: 'Day -4', actual: 3790000, achieveRate: 99, primaryReason: 'Import customs duty local fast-tracks.' },
      { day: 'Day -3', actual: 3950000, achieveRate: 103, primaryReason: 'Export freight carrier bulk payment release.' },
      { day: 'Day -2', actual: 3420000, achieveRate: 90, primaryReason: 'Severe load-shedding disrupting regional merchant POS.' },
      { day: 'Day -1', actual: 3880000, achieveRate: 102, primaryReason: 'Corporate payroll and agency cash-outs.' }
    ],
    predictedDrivers: {
      increase: 'Port clearing activities and high import freight cash flow.',
      decrease: 'Industrial power rationing and logistical delays.'
    }
  },
  'Sylhet Division': {
    avgDailyTarget: 2200000,
    historicalPerformance: [
      { day: 'Day -5', actual: 2310000, achieveRate: 105, primaryReason: 'Strong expatriate remittance flows via secure API links.' },
      { day: 'Day -4', actual: 2150000, achieveRate: 97, primaryReason: 'Local tea garden labor disbursement delays.' },
      { day: 'Day -3', actual: 2450000, achieveRate: 111, primaryReason: 'Eid festival home remittance peaks.' },
      { day: 'Day -2', actual: 1980000, achieveRate: 90, primaryReason: 'Border surveillance heightened slowing tourist cash flows.' },
      { day: 'Day -1', actual: 2280000, achieveRate: 103, primaryReason: 'Expat real-estate acquisition transfers.' }
    ],
    predictedDrivers: {
      increase: 'Sudden remittance payouts and land transactions.',
      decrease: 'Border trade restrictions and remittance channel blockages.'
    }
  },
  'Rajshahi Division': {
    avgDailyTarget: 1800000,
    historicalPerformance: [
      { day: 'Day -5', actual: 1720000, achieveRate: 95, primaryReason: 'Silk retail hub and seasonal mango harvests trade expansion.' },
      { day: 'Day -4', actual: 1850000, achieveRate: 102, primaryReason: 'University semester tuition fee processing surge.' },
      { day: 'Day -3', actual: 1910000, achieveRate: 106, primaryReason: 'Inter-district transport hub utility invoice clearing.' },
      { day: 'Day -2', actual: 1610000, achieveRate: 89, primaryReason: 'Heavy rainfall in agricultural fields blocking physical transport.' },
      { day: 'Day -1', actual: 1780000, achieveRate: 98, primaryReason: 'Dynamic agricultural cold-storage business credits.' }
    ],
    predictedDrivers: {
      increase: 'Agricultural cash cycle payments and student loan disbursements.',
      decrease: 'Intermittent power supply to local digital booths.'
    }
  }
};

const INITIAL_AGENTS = [
  { id: 'AGT-8821', agentName: 'Mamun Telecom (Dhaka City)', division: 'Dhaka Division', district: 'Dhaka City', eMoneyFloat: 280000, physicalCash: 45000, dailyInflow: 350000, dailyOutflow: 390000, status: 'Optimal' },
  { id: 'AGT-4402', agentName: 'Sarkar Enterprises (Agrabad)', division: 'Chittagong Division', district: 'Chittagong City', eMoneyFloat: 12000, physicalCash: 310000, dailyInflow: 580000, dailyOutflow: 180000, status: 'E-Money Depleted' },
  { id: 'AGT-7731', agentName: 'Sylhet International Trading', division: 'Sylhet Division', district: 'Sylhet City', eMoneyFloat: 490000, physicalCash: 5000, dailyInflow: 90000, dailyOutflow: 480000, status: 'Cash Constrained' },
  { id: 'AGT-1205', agentName: 'Rajshahi Agro Agents', division: 'Rajshahi Division', district: 'Sirajganj', eMoneyFloat: 150000, physicalCash: 160000, dailyInflow: 220000, dailyOutflow: 210000, status: 'Optimal' },
  { id: 'AGT-9012', agentName: 'Khulna Boyra MFS Cabin', division: 'Khulna Division', district: 'Khulna City', eMoneyFloat: 4000, physicalCash: 180000, dailyInflow: 290000, dailyOutflow: 110000, status: 'E-Money Depleted' }
];

const NAMES_POOL = ['Tahsan Khan', 'Anika Tabassum', 'Abrar Fahim', 'Tasnim Alam', 'Sajid Islam', 'Nusrat Jahan', 'Faisal Ahmed', 'Sadia Chowdhury', 'Mehedi Hasan', 'Imran Hossain'];
const DEVICES_POOL = ['Samsung Galaxy S23 Ultra', 'Rooted OnePlus 9 Pro', 'iPhone 14 Pro Max', 'Emulator Genymotion Build-90', 'Infinix Note 12', 'Jailbroken iPad mini'];
const IPS_POOL = ['103.218.24.11', '180.234.12.80', '103.55.108.2', '202.4.98.5', '103.82.172.9', '103.112.54.43'];

export default function App() {
  const [transactions, setTransactions] = useState(INITIAL_TRANSACTIONS);
  const [activeTab, setActiveTab] = useState('dashboard'); // dashboard, predictiveKpi, spatialMap, profiles, rules, vault
  
  // Geographical Selection States
  const [selectedDivision, setSelectedDivision] = useState('Dhaka Division');
  const [selectedDistrict, setSelectedDistrict] = useState('Dhaka City');
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedTxIdForAnalysis, setSelectedTxIdForAnalysis] = useState('BK-TX-9921');
  const [otpInputs, setOtpInputs] = useState({});
  const [toast, setToast] = useState({ show: false, message: '', type: 'info' });

  // S-FIS Systems States
  const [liveStreamActive, setLiveStreamActive] = useState(true);
  const [liveTps, setLiveTps] = useState(84.5);
  const [smoteMultiplier, setSmoteMultiplier] = useState(1.0);
  const [syntheticRecordCount, setSyntheticRecordCount] = useState(1420);

  // Dynamic District level flow overrides
  const [districtsState, setDistrictsState] = useState(DISTRICTS_DATA);
  const [agents, setAgents] = useState(INITIAL_AGENTS);
  
  // Custom Dynamic KPI target state & historical data injections
  const [historicalKpiDataState, setHistoricalKpiDataState] = useState(INITIAL_KPI_DATA);
  const [customIncentiveBoost, setCustomIncentiveBoost] = useState(10); 
  const [predictiveHorizonDays, setPredictiveHorizonDays] = useState(5);

  // Rules Parameters
  const [ruleVelocityThreshold, setRuleVelocityThreshold] = useState(100000);
  const [aiConfidenceCutoff, setAiConfidenceCutoff] = useState(450);
  const [ruleHighRiskIMSIAction, setRuleHighRiskIMSIAction] = useState('Hold');

  // File Upload states
  const [uploadedFiles, setUploadedFiles] = useState([]);
  const [isDragging, setIsDragging] = useState(false);
  const fileInputRef = useRef(null);

  // Safe geographical selection mapper
  const handleDivisionChange = (divisionName) => {
    setSelectedDivision(divisionName);
    const districts = Object.keys(districtsState[divisionName] || {});
    if (districts.length > 0) {
      setSelectedDistrict(districts[0]);
    }
  };

  const showToast = (message, type = 'info') => {
    setToast({ show: true, message, type });
    setTimeout(() => setToast({ show: false, message: '', type: 'info' }), 4000);
  };

  useEffect(() => {
    let intervalId;
    if (liveStreamActive) {
      intervalId = setInterval(() => {
        const divisionsList = Object.keys(districtsState);
        const randomDiv = divisionsList[Math.floor(Math.random() * divisionsList.length)];
        const districtsList = Object.keys(districtsState[randomDiv]);
        const randomDist = districtsList[Math.floor(Math.random() * districtsList.length)];

        const amount = Math.floor(2000 + Math.random() * 220000);
        const name = NAMES_POOL[Math.floor(Math.random() * NAMES_POOL.length)];
        const device = DEVICES_POOL[Math.floor(Math.random() * DEVICES_POOL.length)];
        const isRooted = device.toLowerCase().includes('rooted') || device.toLowerCase().includes('emulator') || device.toLowerCase().includes('jailbroken');
        const isHighRiskAmount = amount > ruleVelocityThreshold;
        
        let score = 980 - Math.floor(Math.random() * 150);
        if (isRooted) score -= 400;
        if (isHighRiskAmount) score -= 250;
        score = Math.max(100, Math.min(1000, score));

        let finalRoute = 'Normal Fast-Track';
        let finalStatus = 'Settled';
        let reason = 'Approved instantaneously by bKash S-FIS Automated Speed Gate.';

        if (score < aiConfidenceCutoff) {
          if (ruleHighRiskIMSIAction === 'Block' && isRooted) {
            finalRoute = 'Hard Anti-Fraud Lock';
            finalStatus = 'Blocked & Flagged';
            reason = 'Critical security threat. Device is rooted/emulated, violating hardware sandbox.';
          } else {
            finalRoute = 'Suspense Account Hold';
            finalStatus = 'In Suspense Account';
            reason = `Anomalous transaction value. Held in suspense pool pending biometrics validation.`;
          }
        }

        const newTx = {
          id: `BK-TX-${Math.floor(1000 + Math.random() * 9000)}`,
          name,
          amount,
          time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' }),
          type: amount > 150000 ? 'Merchant Pay' : amount > 80000 ? 'Agent Cash-Out' : 'bKash SendMoney',
          score,
          route: finalRoute,
          status: finalStatus,
          division: randomDiv,
          district: randomDist,
          reason,
          ip: IPS_POOL[Math.floor(Math.random() * IPS_POOL.length)],
          device,
          imsiVelocity: isRooted ? 'High Velocity (IMSI swap registered)' : 'Stable',
          location: `${randomDist}, ${randomDiv.replace(' Division', '')}`,
          networkTower: `BTS-${randomDist.slice(0,3).toUpperCase()}-${Math.floor(1 + Math.random() * 9)}`,
          timestamp: Date.now()
        };

        setTransactions(prev => [newTx, ...prev.slice(0, 39)]);
        
        // Dynamically adjust real-time District flows based on simulation
        setDistrictsState(prev => {
          const divObj = { ...prev[randomDiv] };
          if (divObj[randomDist]) {
            const currentDist = { ...divObj[randomDist] };
            if (newTx.type === 'Agent Cash-Out') {
              currentDist.cashOut += amount;
            } else {
              currentDist.cashIn += amount;
            }
            const difference = currentDist.cashIn - currentDist.cashOut;
            currentDist.status = difference < -300000 ? 'Critical' : (difference < -100000 ? 'Warning' : 'Good');
            divObj[randomDist] = currentDist;
          }
          return { ...prev, [randomDiv]: divObj };
        });

        // Dynamically adjust real-time Agent liquidity balance based on stream
        setAgents(prevAgents => prevAgents.map(agent => {
          if (agent.district === randomDist) {
            const isCashOut = newTx.type === 'Agent Cash-Out';
            let newEMoney = agent.eMoneyFloat;
            let newCash = agent.physicalCash;
            let dailyIn = agent.dailyInflow;
            let dailyOut = agent.dailyOutflow;

            if (isCashOut) {
              newEMoney += amount;
              newCash -= amount;
              dailyIn += amount;
            } else {
              newEMoney -= amount;
              newCash += amount;
              dailyOut += amount;
            }

            let status = 'Optimal';
            if (newEMoney < 25000) status = 'E-Money Depleted';
            else if (newCash < 20000) status = 'Cash Constrained';

            return {
              ...agent,
              eMoneyFloat: Math.max(0, newEMoney),
              physicalCash: Math.max(0, newCash),
              dailyInflow: dailyIn,
              dailyOutflow: dailyOut,
              status
            };
          }
          return agent;
        }));

        setLiveTps(prev => Math.max(60, Math.min(160, Number((prev + (Math.random() * 10 - 5)).toFixed(1)))));
        if (score < 400) {
          setSyntheticRecordCount(c => c + Math.floor(1 * smoteMultiplier));
        }
      }, 4000);
    }
    return () => clearInterval(intervalId);
    // REMOVED districtsState from dependencies to fix the endless interval reconstruction loop
  }, [liveStreamActive, ruleVelocityThreshold, aiConfidenceCutoff, ruleHighRiskIMSIAction, smoteMultiplier]);

  const statistics = useMemo(() => {
    const totalCount = transactions.length;
    const settled = transactions.filter(t => t.status === 'Settled');
    const suspense = transactions.filter(t => t.status === 'In Suspense Account' || t.status === 'Pending OTP Validation');
    const totalVolume = transactions.reduce((acc, curr) => acc + curr.amount, 0);
    const averageRisk = totalCount ? Math.round(transactions.reduce((acc, curr) => acc + curr.score, 0) / totalCount) : 0;
    const falsePositives = transactions.filter(t => t.score < aiConfidenceCutoff && t.status === 'Settled').length;
    const estimatedFpr = totalCount ? ((falsePositives / totalCount) * 100).toFixed(2) : '0.00';

    return {
      totalCount,
      settledCount: settled.length,
      suspenseCount: suspense.length,
      totalVolume,
      averageRisk,
      fpr: estimatedFpr
    };
  }, [transactions, aiConfidenceCutoff]);

  const divisionFlows = useMemo(() => {
    const flows = {};
    Object.keys(districtsState).forEach(div => {
      let totalIn = 0;
      let totalOut = 0;
      Object.keys(districtsState[div]).forEach(dist => {
        totalIn += districtsState[div][dist].cashIn;
        totalOut += districtsState[div][dist].cashOut;
      });
      const difference = totalIn - totalOut;
      let flowStatus = 'good';
      if (difference < -500000) {
        flowStatus = 'critical'; 
      } else if (difference < -150000) {
        flowStatus = 'warning';
      }
      flows[div] = { inflow: totalIn, outflow: totalOut, difference, status: flowStatus };
    });
    return flows;
  }, [districtsState]);

  const predictiveKpiCalculations = useMemo(() => {
    const divModel = historicalKpiDataState[selectedDivision] || {
      avgDailyTarget: 2000000,
      historicalPerformance: [
        { day: 'Day -1', actual: 1950000, achieveRate: 97, primaryReason: 'Base estimation' }
      ],
      predictedDrivers: { increase: 'General marketing expansion', decrease: 'Infrastructural network delays' }
    };
    const boostMultiplier = 1 + (customIncentiveBoost / 100);
    const forecasts = [];
    const lastActual = divModel.historicalPerformance.length > 0 
      ? divModel.historicalPerformance[divModel.historicalPerformance.length - 1].actual 
      : divModel.avgDailyTarget;
    const targetBase = divModel.avgDailyTarget;

    for (let i = 1; i <= predictiveHorizonDays; i++) {
      const cyclicalVariance = Math.sin(i * 1.5) * 180000;
      const predictedActual = Math.round((lastActual + cyclicalVariance) * boostMultiplier);
      const achievement = Math.round((predictedActual / targetBase) * 100);
      
      let trendReason = "Standard seasonal distribution match.";
      if (achievement > 105) {
        trendReason = `Increased by custom ${customIncentiveBoost}% agent incentive programs & high market demand.`;
      } else if (achievement < 95) {
        trendReason = `Potential drop due to localized agricultural cash extraction or network latency.`;
      }

      forecasts.push({
        day: `Future Day +${i}`,
        predictedActual,
        expectedAchieveRate: achievement,
        trendReason
      });
    }

    return {
      targetBase,
      forecasts,
      historical: divModel.historicalPerformance,
      drivers: divModel.predictedDrivers
    };
  }, [historicalKpiDataState, selectedDivision, customIncentiveBoost, predictiveHorizonDays]);

  const handleFileUpload = (e) => {
    const files = e.target.files ? Array.from(e.target.files) : [];
    processFiles(files);
  };

  const processFiles = (files) => {
    if (!files.length) return;
    
    files.forEach(file => {
      const reader = new FileReader();
      const fileExt = file.name.split('.').pop().toLowerCase();
      
      reader.onload = (event) => {
        const text = event.target.result;
        let parsedSuccess = false;
        
        try {
          if (fileExt === 'csv') {
            const rows = text.split('\n');
            if (rows.length > 1) {
              const headers = rows[0].split(',').map(h => h.trim().toLowerCase());
              const parsedRecords = [];
              for (let i = 1; i < rows.length; i++) {
                if (!rows[i].trim()) continue;
                const cols = rows[i].split(',').map(c => c.trim());
                const record = {};
                headers.forEach((h, index) => {
                  record[h] = cols[index];
                });
                parsedRecords.push(record);
              }

              if (parsedRecords.length > 0) {
                const targetDivision = selectedDivision;
                const newPerformanceNodes = parsedRecords.map((rec, index) => ({
                  day: rec.day || rec.date || `Uploaded Day -${index + 1}`,
                  actual: Number(rec.actual || rec.cashin || rec.amount || 2000000),
                  achieveRate: Number(rec.rate || rec.achieve || 100),
                  primaryReason: rec.reason || rec.driver || 'Imported from CSV operational file upload.'
                }));

                setHistoricalKpiDataState(prev => {
                  const updatedModel = prev[targetDivision] ? { ...prev[targetDivision] } : {
                    avgDailyTarget: 2500000,
                    historicalPerformance: [],
                    predictedDrivers: { increase: 'CSV feed upload', decrease: 'Adverse risk adjustments' }
                  };
                  updatedModel.historicalPerformance = [...updatedModel.historicalPerformance, ...newPerformanceNodes];
                  return { ...prev, [targetDivision]: updatedModel };
                });
                parsedSuccess = true;
              }
            }
          } else {
            const randomTargets = [2800000, 3100000, 4200000, 1950000];
            const randomActuals = [2950000, 3050000, 4400000, 1850000];
            const chosenTarget = randomTargets[Math.floor(Math.random() * randomTargets.length)];
            const chosenActual = randomActuals[Math.floor(Math.random() * randomActuals.length)];

            const newPerformance = {
              day: 'Document Sync Node',
              actual: chosenActual,
              achieveRate: Math.round((chosenActual / chosenTarget) * 100),
              primaryReason: `Parsed catalyst tokens from uploaded ${fileExt.toUpperCase()} document: "${file.name}"`
            };

            setHistoricalKpiDataState(prev => {
              const updatedModel = prev[selectedDivision] ? { ...prev[selectedDivision] } : {
                avgDailyTarget: chosenTarget,
                historicalPerformance: [],
                predictedDrivers: { increase: 'Document token capture', decrease: 'Uncertain regulatory factors' }
              };
              updatedModel.historicalPerformance = [...updatedModel.historicalPerformance, newPerformance];
              return { ...prev, [selectedDivision]: updatedModel };
            });
            parsedSuccess = true;
          }

          if (parsedSuccess) {
            setUploadedFiles(prev => [...prev, {
              name: file.name,
              size: (file.size / 1024).toFixed(1) + ' KB',
              type: fileExt.toUpperCase(),
              timestamp: new Date().toLocaleTimeString()
            }]);
            showToast(`Document "${file.name}" analyzed successfully! S-FIS Predictor recalibrated.`, 'success');
          } else {
            showToast(`Format mismatch or empty records in ${file.name}`, 'error');
          }

        } catch (err) {
          showToast(`Parser execution error in document processing.`, 'error');
        }
      };

      if (fileExt === 'csv' || fileExt === 'txt') {
        reader.readAsText(file);
      } else {
        reader.readAsBinaryString(file);
      }
    });
  };

  const handleDragOver = (e) => {
    e.preventDefault();
    setIsDragging(true);
  };

  const handleDragLeave = () => {
    setIsDragging(false);
  };

  const handleDrop = (e) => {
    e.preventDefault();
    setIsDragging(false);
    const files = e.dataTransfer.files ? Array.from(e.dataTransfer.files) : [];
    processFiles(files);
  };

  const handleInjectManualPerformance = (e) => {
    e.preventDefault();
    const actualVal = parseInt(e.target.manualActual.value, 10);
    const targetVal = parseInt(e.target.manualTarget.value, 10);
    const reasonStr = e.target.manualReason.value;

    if (isNaN(actualVal) || isNaN(targetVal)) {
      showToast('Please enter valid numeric Target and Actual BDT bounds.', 'error');
      return;
    }

    const rate = Math.round((actualVal / targetVal) * 100);
    const newRecord = {
      day: `Injected Node #${Date.now().toString().slice(-4)}`,
      actual: actualVal,
      achieveRate: rate,
      primaryReason: reasonStr || 'Operator-submitted historical marker.'
    };

    setHistoricalKpiDataState(prev => {
      const updatedModel = prev[selectedDivision] ? { ...prev[selectedDivision] } : {
        avgDailyTarget: targetVal,
        historicalPerformance: [],
        predictedDrivers: { increase: 'Operator calibration', decrease: 'Local market stress' }
      };
      updatedModel.historicalPerformance = [...updatedModel.historicalPerformance, newRecord];
      return { ...prev, [selectedDivision]: updatedModel };
    });

    e.target.reset();
    showToast(`Injected historic calibration marker into ${selectedDivision}!`, 'success');
  };

  const triggerAgentRefloat = (agentId) => {
    setAgents(prev => prev.map(a => {
      if (a.id === agentId) {
        showToast(`Refloating operating balances for ${a.agentName}...`, 'success');
        return {
          ...a,
          eMoneyFloat: a.eMoneyFloat + 150000,
          physicalCash: a.physicalCash + 100000,
          status: 'Optimal'
        };
      }
      return a;
    }));
  };

  const resolveSuspense = (txId, clearSuccess) => {
    setTransactions(prev => prev.map(t => {
      if (t.id === txId) {
        return {
          ...t,
          status: clearSuccess ? 'Settled' : 'Blocked & Flagged',
          route: clearSuccess ? 'Normal Fast-Track' : 'Hard Anti-Fraud Lock',
          reason: clearSuccess 
            ? 'Manually verified via secure OTP challenges.' 
            : 'Failsafe override activated. Target wallet blacklisted and frozen.'
        };
      }
      return t;
    }));
    showToast(clearSuccess ? 'Transaction cleared and routed.' : 'Anomalous funds locked within core vault.', clearSuccess ? 'success' : 'error');
  };

  const downloadSampleTemplate = () => {
    const csvContent = "data:text/csv;charset=utf-8,Day,Actual,Target,Rate,Reason\n" +
      "Day -1,4500000,4000000,112,High transport terminal activity\n" +
      "Day -2,3800000,4000000,95,Localized telecommunications tower failure\n" +
      "Day -3,4900000,4200000,116,Regional festival crop settlement disbursements\n";
    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", "bkash_s_fis_kpi_template.csv");
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    showToast('Downloaded sample CSV dataset schema!', 'success');
  };

  const filteredTransactions = useMemo(() => {
    return transactions.filter(t => {
      const matchDiv = t.division === selectedDivision;
      const matchSearch = t.id.toLowerCase().includes(searchQuery.toLowerCase()) || 
                          t.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
                          t.district.toLowerCase().includes(searchQuery.toLowerCase());
      return matchDiv && matchSearch;
    });
  }, [transactions, selectedDivision, searchQuery]);

  const activeAnalyzedTx = useMemo(() => {
    return transactions.find(t => t.id === selectedTxIdForAnalysis) || transactions[0];
  }, [transactions, selectedTxIdForAnalysis]);

  // Safer helper variables for metrics to avoid NaN displays
  const focusedDistrictData = districtsState[selectedDivision]?.[selectedDistrict];
  const cashInflow = focusedDistrictData?.cashIn || 0;
  const cashOutflow = focusedDistrictData?.cashOut || 0;
  const balanceDelta = cashInflow - cashOutflow;

  return (
    <div className="min-h-screen bg-[#060913] text-slate-100 font-sans p-3 md:p-6 selection:bg-[#E2125B] selection:text-white">
      
      {/* Toast Alert Notification System */}
      {toast.show && (
        <div className={`fixed bottom-6 right-6 z-50 flex items-center gap-3 px-5 py-4 rounded-2xl border shadow-2xl transition-all duration-300 transform translate-y-0 ${
          toast.type === 'success' ? 'bg-emerald-950/95 border-emerald-500/40 text-emerald-300' :
          toast.type === 'error' ? 'bg-rose-950/95 border-rose-500/40 text-rose-300' :
          'bg-slate-900/95 border-rose-500/20 text-rose-200'
        }`}>
          <div className="w-2.5 h-2.5 rounded-full bg-current animate-ping"></div>
          <p className="text-xs font-semibold tracking-wide font-mono">{toast.message}</p>
        </div>
      )}

      {/* HEADER SECTION */}
      <header className="mb-6 bg-slate-900/60 border border-slate-800/80 p-5 rounded-3xl flex flex-col lg:flex-row justify-between items-start lg:items-center gap-6 backdrop-blur-md">
        <div className="flex items-center gap-4">
          <div className="relative">
            <div className="w-12 h-12 rounded-2xl bg-[#E2125B] flex items-center justify-center shadow-lg shadow-rose-950/40">
              <Shield className="w-7 h-7 text-white" />
            </div>
            <span className="absolute -top-1 -right-1 w-5 h-5 bg-emerald-500 border-2 border-slate-950 rounded-full flex items-center justify-center text-[8px] font-black animate-pulse text-slate-950">LIVE</span>
          </div>

          <div>
            <div className="flex flex-wrap items-center gap-2">
              <span className="bg-[#E2125B]/10 text-[#E2125B] text-[9px] px-3 py-0.5 rounded-full font-bold uppercase tracking-wider border border-[#E2125B]/20">
                bKash Sentinel S-FIS Suite
              </span>
              <span className="bg-slate-800 text-slate-400 text-[9px] px-2.5 py-0.5 rounded-full font-mono uppercase tracking-widest">
                GEOGRAPHIC MATCH + PREDICTIVE UPLOADS
              </span>
            </div>
            <h1 className="text-xl md:text-2xl font-black mt-1 bg-gradient-to-r from-white via-slate-100 to-rose-200 bg-clip-text text-transparent">
              Sentinel Command & Forecast Engine <span className="text-[#E2125B] text-sm font-bold">V6.0 Premium</span>
            </h1>
            <p className="text-[11px] text-slate-400 mt-0.5 font-mono">
              64-District High-Tech Heatmapping, Automated Document Prediction Core, and Cashflow Balance S-FIS Pipelines.
            </p>
          </div>
        </div>

        <div className="flex flex-wrap items-center gap-3 w-full lg:w-auto">
          <button 
            onClick={() => setLiveStreamActive(!liveStreamActive)}
            className={`px-4 py-2 rounded-xl text-xs font-mono font-bold transition-all flex items-center gap-2 border ${
              liveStreamActive 
                ? 'bg-emerald-950/30 text-emerald-400 border-emerald-500/30' 
                : 'bg-rose-950/30 text-rose-400 border-rose-500/30'
            }`}
          >
            <span className={`w-2 h-2 rounded-full ${liveStreamActive ? 'bg-emerald-400 animate-ping' : 'bg-rose-500'}`}></span>
            {liveStreamActive ? 'LIVE TRANSACTIONS ACTIVE' : 'STREAM INTERCEPTED'}
          </button>

          <button 
            onClick={() => {
              setSmoteMultiplier(prev => prev + 0.5);
              setSyntheticRecordCount(prev => prev + 350);
              showToast('SMOTE Over-sampling multiplier scaled up!', 'success');
            }}
            className="bg-[#E2125B] hover:bg-[#c10e4e] text-white px-4 py-2 rounded-xl text-xs font-mono font-bold transition-all shadow-md"
          >
            ⚡ OPTIMIZE SMOTE SETTINGS
          </button>
        </div>
      </header>

      {/* CORE HIGH-TECH METRICS */}
      <section className="grid grid-cols-2 lg:grid-cols-6 gap-4 mb-6">
        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">LIVE THROUGHPUT</span>
            <Activity className="w-4 h-4 text-emerald-400 animate-pulse" />
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-emerald-400">{liveTps} <span className="text-xs">TPS</span></div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">S-FIS Processing Speed: ~7.1ms</p>
          </div>
        </div>

        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">FALSE POSITIVE (FPR)</span>
            <Shield className="w-4 h-4 text-rose-500" />
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-rose-400">{statistics.fpr}%</div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">Class imbalance margins</p>
          </div>
        </div>

        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">SMOTE BALANCE</span>
            <Database className="w-4 h-4 text-purple-400" />
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-cyan-400">1 : {Math.round(40 / smoteMultiplier)}</div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">{syntheticRecordCount} balanced rows</p>
          </div>
        </div>

        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">SUSPENSE ESCROW</span>
            <Cpu className="w-4 h-4 text-amber-400" />
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-amber-400">{statistics.suspenseCount} Hold</div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">Quarantined inside security pool</p>
          </div>
        </div>

        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">IMPORTED SETS</span>
            <FileText className="w-4 h-4 text-cyan-400" />
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-cyan-400">{uploadedFiles.length} files</div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">Uploaded operational CSVs/spreadsheets</p>
          </div>
        </div>

        <div className="bg-slate-900/40 border border-slate-800/80 p-4 rounded-2xl flex flex-col justify-between">
          <div className="flex justify-between items-center mb-1">
            <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider font-bold">FLOW PRESSURE</span>
            <span className="w-1.5 h-1.5 rounded-full bg-rose-500 animate-ping"></span>
          </div>
          <div>
            <div className="text-2xl font-black font-mono text-rose-500">
              {Object.values(divisionFlows).filter(f => f.status === 'critical').length} Deficits
            </div>
            <p className="text-[9px] text-slate-500 font-mono mt-1">High-risk withdraw clusters</p>
          </div>
        </div>
      </section>

      {/* NAVIGATION TABS */}
      <nav className="flex flex-wrap gap-2 mb-6 border-b border-slate-800/60 pb-3">
        <button 
          onClick={() => setActiveTab('dashboard')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'dashboard' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          📊 Live Feed & Terminal
        </button>
        <button 
          onClick={() => setActiveTab('spatialMap')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'spatialMap' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          🗺️ Interactive District Heatmap
        </button>
        <button 
          onClick={() => setActiveTab('predictiveKpi')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'predictiveKpi' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          📈 Document Predictor & KPI Hub
        </button>
        <button 
          onClick={() => setActiveTab('profiles')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'profiles' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          👤 Device Profiler Workspace
        </button>
        <button 
          onClick={() => setActiveTab('rules')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'rules' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          ⚙️ Security Overrides
        </button>
        <button 
          onClick={() => setActiveTab('vault')}
          className={`px-4 py-2 rounded-xl font-mono text-xs font-bold transition-all flex items-center gap-2 ${
            activeTab === 'vault' ? 'bg-[#E2125B] text-white shadow-lg shadow-rose-950/20' : 'bg-slate-900/50 hover:bg-slate-800/50 text-slate-400'
          }`}
        >
          🔒 Escrow Suspense Vault ({statistics.suspenseCount})
        </button>
      </nav>

      {/* MAIN CONTAINER */}
      <div className="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start">

        {/* TAB 1: EXECUTIVE LIVE FEED DASHBOARD */}
        {activeTab === 'dashboard' && (
          <>
            {}
            <div className="xl:col-span-4 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md space-y-6">
              <div>
                <h3 className="text-sm font-bold text-slate-100 font-mono mb-2 flex items-center gap-2">
                  <span className="w-2.5 h-2.5 rounded-full bg-[#E2125B]"></span>
                  Target Region Surges
                </h3>
                <p className="text-[11px] text-slate-400 font-mono">
                  Select surveillance focal point (Division & District) below to filter operations:
                </p>
              </div>

              <div className="grid grid-cols-2 gap-3">
                <div>
                  <label className="block text-[9px] font-bold text-slate-400 uppercase tracking-wider font-mono mb-1.5">Division Focus</label>
                  <select 
                    value={selectedDivision}
                    onChange={(e) => handleDivisionChange(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 px-3 py-2.5 rounded-xl text-xs text-slate-200 focus:outline-none focus:border-[#E2125B] font-mono"
                  >
                    {Object.keys(districtsState).map(div => (
                      <option key={div} value={div}>{div}</option>
                    ))}
                  </select>
                </div>

                <div>
                  <label className="block text-[9px] font-bold text-slate-400 uppercase tracking-wider font-mono mb-1.5">District Focus</label>
                  <select 
                    value={selectedDistrict}
                    onChange={(e) => setSelectedDistrict(e.target.value)}
                    className="w-full bg-slate-950 border border-slate-800 px-3 py-2.5 rounded-xl text-xs text-slate-200 focus:outline-none focus:border-[#E2125B] font-mono"
                  >
                    {Object.keys(districtsState[selectedDivision] || {}).map(dist => (
                      <option key={dist} value={dist}>{dist}</option>
                    ))}
                  </select>
                </div>
              </div>

              {/* Dynamic flow telemetry for chosen district */}
              <div className="p-4 bg-slate-950/60 border border-slate-800 rounded-2xl space-y-3 font-mono">
                <span className="text-[10px] text-[#E2125B] font-bold uppercase tracking-widest block">District Real-time telemetry</span>
                <h4 className="text-xs font-bold text-white">{selectedDistrict} Overview</h4>
                
                <div className="space-y-2 text-xs">
                  <div className="flex justify-between">
                    <span className="text-slate-400">Cash Inflow:</span>
                    <span className="text-emerald-400 font-bold">
                      {cashInflow.toLocaleString()} BDT
                    </span>
                  </div>
                  <div className="flex justify-between">
                    <span className="text-slate-400">Cash Outflow:</span>
                    <span className="text-rose-400 font-bold">
                      {cashOutflow.toLocaleString()} BDT
                    </span>
                  </div>
                  <div className="flex justify-between border-t border-slate-800/80 pt-2 font-black">
                    <span className="text-slate-200">Balance Delta:</span>
                    <span className={balanceDelta >= 0 ? "text-emerald-400" : "text-rose-500"}>
                      {balanceDelta.toLocaleString()} BDT
                    </span>
                  </div>
                </div>
              </div>

              {/* XGBoost Weight Coefficients */}
              <div className="pt-2 border-t border-slate-800/60">
                <span className="text-[10px] text-slate-500 font-mono uppercase tracking-wider block mb-3 font-bold">XGBoost Feature Weight Coefficients</span>
                <div className="space-y-2">
                  <div>
                    <div className="flex justify-between text-[10px] font-mono mb-1">
                      <span>Sim Swap Interval Velocity</span>
                      <span className="text-[#E2125B]">42.8%</span>
                    </div>
                    <div className="h-1 bg-slate-950 rounded-full overflow-hidden">
                      <div className="h-full bg-[#E2125B] rounded-full" style={{ width: '42.8%' }}></div>
                    </div>
                  </div>

                  <div>
                    <div className="flex justify-between text-[10px] font-mono mb-1">
                      <span>Geometric Jump Speed (GPS Velocity)</span>
                      <span className="text-[#E2125B]">28.4%</span>
                    </div>
                    <div className="h-1 bg-slate-950 rounded-full overflow-hidden">
                      <div className="h-full bg-[#E2125B] rounded-full" style={{ width: '28.4%' }}></div>
                    </div>
                  </div>

                  <div>
                    <div className="flex justify-between text-[10px] font-mono mb-1">
                      <span>USSD Transaction Interval</span>
                      <span className="text-emerald-400">14.1%</span>
                    </div>
                    <div className="h-1 bg-slate-950 rounded-full overflow-hidden">
                      <div className="h-full bg-emerald-500 rounded-full" style={{ width: '14.1%' }}></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            {/* Main Streaming Live Feed */}
            <div className="xl:col-span-8 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md">
              <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 mb-4 pb-4 border-b border-slate-800/60">
                <div>
                  <h3 className="text-sm font-bold text-slate-100 font-mono flex items-center gap-2">
                    <span className="w-2.5 h-2.5 rounded-full bg-[#E2125B] animate-ping"></span>
                    Live S-FIS Gateway Feed
                  </h3>
                  <p className="text-[10px] text-slate-400 font-mono mt-0.5">Filter focused Division: <strong className="text-[#E2125B]">{selectedDivision}</strong></p>
                </div>

                <div className="flex gap-2 w-full md:w-auto">
                  <input 
                    type="text" 
                    placeholder="Search Wallet ID, Name, District..."
                    value={searchQuery}
                    onChange={(e) => setSearchQuery(e.target.value)}
                    className="bg-slate-950 border border-slate-800 text-slate-200 text-xs px-3 py-2 rounded-xl focus:outline-none font-mono w-full md:w-64"
                  />
                </div>
              </div>

              <div className="overflow-x-auto">
                <table className="w-full text-left text-xs text-slate-300 font-mono">
                  <thead>
                    <tr className="text-[10px] uppercase font-bold text-slate-500 border-b border-slate-800/80">
                      <th className="py-2.5 px-2">Wallet ID / Name</th>
                      <th className="py-2.5 px-2">BDT Amount</th>
                      <th className="py-2.5 px-2">District Node</th>
                      <th className="py-2.5 px-2">S-FIS Index</th>
                      <th className="py-2.5 px-2">Routing Decision</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-slate-800/40">
                    {filteredTransactions.map(tx => (
                      <tr 
                        key={tx.id} 
                        onClick={() => { setSelectedTxIdForAnalysis(tx.id); showToast(`Locking onto hardware profile: ${tx.id}`, 'info'); }}
                        className={`hover:bg-slate-800/20 transition-all cursor-pointer ${selectedTxIdForAnalysis === tx.id ? 'bg-slate-800/30' : ''}`}
                      >
                        <td className="py-3 px-2">
                          <span className="font-extrabold text-white block">{tx.id}</span>
                          <span className="text-[10px] text-slate-500 block max-w-[150px] truncate">{tx.name}</span>
                        </td>
                        <td className="py-3 px-2 font-bold text-[#E2125B]">{tx.amount.toLocaleString()} BDT</td>
                        <td className="py-3 px-2 font-semibold text-slate-300">{tx.district}</td>
                        <td className="py-3 px-2">
                          <span className={`font-bold ${tx.score >= 750 ? 'text-emerald-400' : tx.score >= aiConfidenceCutoff ? 'text-amber-400' : 'text-rose-500'}`}>{tx.score}</span>
                        </td>
                        <td className="py-3 px-2">
                          <span className={`inline-block px-2 py-0.5 rounded-full text-[9px] border ${
                            tx.route === 'Normal Fast-Track' ? 'bg-emerald-950/20 text-emerald-400 border-emerald-500/20' :
                            tx.route === 'Suspense Account Hold' ? 'bg-amber-950/20 text-amber-400 border-amber-500/20' :
                            'bg-rose-950/20 text-rose-500 border-rose-500/20'
                          }`}>{tx.route}</span>
                        </td>
                      </tr>
                    ))}
                    {filteredTransactions.length === 0 && (
                      <tr>
                        <td colSpan="5" className="text-center py-8 text-slate-500">No active streaming anomalies detected inside {selectedDivision}.</td>
                      </tr>
                    )}
                  </tbody>
                </table>
              </div>
            </div>
          </>
        )}

        {/* TAB 2: DETAILED BANGLADESH MAP WITH DISTRICT DRILLDOWN */}
        {activeTab === 'spatialMap' && (
          <div className="xl:col-span-12 space-y-6">
            <div className="bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md">
              <div className="flex flex-col lg:flex-row justify-between items-start lg:items-center gap-4 mb-6">
                <div>
                  <h2 className="text-base font-extrabold text-white font-mono flex items-center gap-2">
                    🗺️ Interactive Bangladesh Division & District Surveillance Map
                  </h2>
                  <p className="text-xs text-slate-400 font-mono mt-1">
                    Visual match alignment of geographical anchors. Click Division territory segments to expand District nodes.
                  </p>
                </div>
                <div className="flex items-center gap-3">
                  <span className="flex items-center gap-1.5 text-xs text-emerald-400 font-mono"><span className="w-2 h-2 bg-emerald-500 rounded-full"></span> Good Flows</span>
                  <span className="flex items-center gap-1.5 text-xs text-amber-400 font-mono"><span className="w-2 h-2 bg-amber-500 rounded-full"></span> Balance Warning</span>
                  <span className="flex items-center gap-1.5 text-xs text-rose-500 font-mono"><span className="w-2 h-2 bg-rose-600 rounded-full"></span> Critical Deficit</span>
                </div>
              </div>

              <div className="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
                
                {/* SVG DESIGNED TO ALIGN WITH BANGLADESH MAP */}
                <div className="lg:col-span-5 bg-[#03060c] border border-slate-800 p-6 rounded-3xl flex flex-col justify-center items-center relative overflow-hidden">
                  <div className="absolute top-4 left-4 text-[9px] font-mono text-slate-500 tracking-widest uppercase">
                    Surveillance Radar Mesh 
                  </div>
                  
                  {/* Decorative compass rose */}
                  <div className="absolute bottom-4 right-4 text-[10px] font-mono text-slate-600">
                    N 23° 40' 60" / E 90° 22' 30"
                  </div>

                  <svg viewBox="0 0 400 500" className="w-full max-w-[340px] h-auto drop-shadow-2xl">
                    <line x1="0" y1="100" x2="400" y2="100" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="0" y1="200" x2="400" y2="200" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="0" y1="300" x2="400" y2="300" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="0" y1="400" x2="400" y2="400" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="100" y1="0" x2="100" y2="500" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="200" y1="0" x2="200" y2="500" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />
                    <line x1="300" y1="0" x2="300" y2="500" stroke="#10192e" strokeWidth="0.5" strokeDasharray="5 5" />

                    {/* 1. Rangpur Division (Top Left) */}
                    <path 
                      d="M60,40 L130,50 L150,110 L100,140 L50,100 Z" 
                      fill={selectedDivision === 'Rangpur Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Rangpur Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Rangpur Division'); showToast('Locked Rangpur surveillance mesh', 'info'); }}
                    />
                    
                    {/* 2. Rajshahi Division (Mid Left) */}
                    <path 
                      d="M50,100 L100,140 L160,150 L165,220 L110,240 L60,170 Z" 
                      fill={selectedDivision === 'Rajshahi Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Rajshahi Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Rajshahi Division'); showToast('Locked Rajshahi surveillance mesh', 'info'); }}
                    />

                    {/* 3. Mymensingh Division (Top Mid) */}
                    <path 
                      d="M130,50 L200,60 L230,120 L180,155 L150,110 Z" 
                      fill={selectedDivision === 'Mymensingh Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Mymensingh Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Mymensingh Division'); showToast('Locked Mymensingh surveillance mesh', 'info'); }}
                    />

                    {/* 4. Sylhet Division (Top Right) */}
                    <path 
                      d="M200,60 L320,50 L340,140 L280,210 L230,120 Z" 
                      fill={selectedDivision === 'Sylhet Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Sylhet Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Sylhet Division'); showToast('Locked Sylhet surveillance mesh', 'info'); }}
                    />

                    {/* 5. Dhaka Division (Center) */}
                    <path 
                      d="M160,150 L230,120 L280,210 L250,300 L180,290 L165,220 Z" 
                      fill={selectedDivision === 'Dhaka Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Dhaka Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Dhaka Division'); showToast('Locked Dhaka surveillance mesh', 'info'); }}
                    />

                    {/* 6. Khulna Division (Bottom Left) */}
                    <path 
                      d="M110,240 L180,290 L170,390 L100,380 L105,290 Z" 
                      fill={selectedDivision === 'Khulna Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Khulna Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Khulna Division'); showToast('Locked Khulna surveillance mesh', 'info'); }}
                    />

                    {/* 7. Barishal Division (Bottom Center) */}
                    <path 
                      d="M180,290 L250,300 L240,390 L195,410 L170,390 Z" 
                      fill={selectedDivision === 'Barishal Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Barishal Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Barishal Division'); showToast('Locked Barishal surveillance mesh', 'info'); }}
                    />

                    {/* 8. Chittagong Division (South East) */}
                    <path 
                      d="M280,210 L340,140 L350,260 L380,350 L340,490 L295,380 L250,300 Z" 
                      fill={selectedDivision === 'Chittagong Division' ? '#E2125B' : MAP_GREEN}
                      fillOpacity={selectedDivision === 'Chittagong Division' ? '0.85' : '0.45'}
                      stroke="#0d241c" strokeWidth="1.5" className="cursor-pointer transition-all hover:fill-rose-600"
                      onClick={() => { handleDivisionChange('Chittagong Division'); showToast('Locked Chittagong surveillance mesh', 'info'); }}
                    />

                    {/* Dynamic District Nodes rendered on top of Map */}
                    {Object.keys(districtsState).map(div => {
                      if (div !== selectedDivision) return null;
                      return Object.keys(districtsState[div]).map(dist => {
                        const info = districtsState[div][dist];
                        let color = '#10b981'; // Good
                        if (info.status === 'Warning') color = '#fbbf24';
                        if (info.status === 'Critical') color = '#ef4444';
                        return (
                          <g key={dist} className="cursor-pointer" onClick={() => { setSelectedDistrict(dist); showToast(`Focus locked: ${dist}`, 'success'); }}>
                            <circle cx={info.longitude} cy={info.latitude} r="6" fill={color} stroke="#0B0F19" strokeWidth="1.5" className="hover:scale-150 transition-all duration-150" />
                            <text x={info.longitude + 8} y={info.latitude + 3} fill="#cbd5e1" fontSize="7" fontFamily="monospace" fontWeight="bold">{dist.replace(" City", "")}</text>
                          </g>
                        );
                      });
                    })}
                  </svg>
                </div>

                {/* DETAILED DISTRICT LEVEL OVERFLOW & NET PRESSURE BREAKDOWN */}
                <div className="lg:col-span-7 space-y-4">
                  <div className="bg-slate-950/80 border border-slate-800 p-4 rounded-2xl">
                    <span className="text-[10px] text-[#E2125B] font-mono uppercase tracking-widest font-black block mb-2">Focused division context</span>
                    <h3 className="text-sm font-bold text-slate-100 font-mono mb-4">
                      {selectedDivision} District Overflow Indices ({Object.keys(districtsState[selectedDivision] || {}).length} active areas)
                    </h3>

                    <div className="grid grid-cols-1 md:grid-cols-2 gap-3 max-h-[380px] overflow-y-auto pr-2">
                      {Object.keys(districtsState[selectedDivision] || {}).map(dist => {
                        const data = districtsState[selectedDivision][dist];
                        const balance = data.cashIn - data.cashOut;
                        const totalTransactions = data.cashIn + data.cashOut;
                        const outflowRatio = ((data.cashOut / (totalTransactions || 1)) * 100).toFixed(0);

                        return (
                          <div 
                            key={dist} 
                            onClick={() => setSelectedDistrict(dist)}
                            className={`p-3 rounded-xl border transition-all cursor-pointer ${
                              selectedDistrict === dist 
                                ? 'bg-[#E2125B]/15 border-[#E2125B]' 
                                : 'bg-slate-900/40 border-slate-800 hover:border-slate-700'
                            }`}
                          >
                            <div className="flex justify-between items-center mb-2">
                              <span className="text-xs font-bold text-white font-mono">{dist}</span>
                              <span className={`text-[8px] font-mono font-bold px-2 py-0.5 rounded uppercase ${
                                data.status === 'Good' ? 'bg-emerald-950 text-emerald-400 border border-emerald-500/20' :
                                data.status === 'Warning' ? 'bg-amber-950 text-amber-400 border border-amber-500/20' :
                                'bg-rose-950 text-rose-400 border border-rose-500/20'
                              }`}>{data.status} overflow</span>
                            </div>

                            <div className="space-y-1.5 text-[10px] font-mono text-slate-400">
                              <div className="flex justify-between">
                                <span>Cash Inflow:</span>
                                <span className="text-emerald-400 font-bold">{(data.cashIn / 1000).toFixed(0)}k BDT</span>
                              </div>
                              <div className="flex justify-between">
                                <span>Cash Outflow:</span>
                                <span className="text-rose-400 font-bold">{(data.cashOut / 1000).toFixed(0)}k BDT</span>
                              </div>
                              <div className="flex justify-between border-t border-slate-800/40 pt-1 font-black text-[11px]">
                                <span>Net Balance:</span>
                                <span className={balance >= 0 ? 'text-emerald-400' : 'text-rose-500'}>
                                  {(balance / 1000).toFixed(0)}k BDT
                                </span>
                              </div>
                            </div>

                            {/* Ratio slider showing percent outflow */}
                            <div className="mt-2 pt-1">
                              <div className="flex justify-between text-[8px] text-slate-500 mb-1">
                                <span>Inflow Share</span>
                                <span>{outflowRatio}% Outflow</span>
                              </div>
                              <div className="h-1 bg-slate-950 rounded-full overflow-hidden">
                                <div 
                                  className={`h-full ${data.status === 'Critical' ? 'bg-rose-600' : (data.status === 'Warning' ? 'bg-amber-500' : 'bg-emerald-500')}`}
                                  style={{ width: `${outflowRatio}%` }}
                                ></div>
                              </div>
                            </div>
                          </div>
                        );
                      })}
                    </div>
                  </div>

                  {/* Active Agents Liquidity Check inside focused district */}
                  <div className="bg-slate-950/80 border border-slate-800 p-4 rounded-2xl">
                    <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold mb-3">District Agent Liquidity Balance Sheet</span>
                    <div className="space-y-2">
                      {agents.filter(a => a.division === selectedDivision).map(agent => (
                        <div key={agent.id} className="p-3 bg-slate-900/40 border border-slate-800/40 rounded-xl flex justify-between items-center text-[11px] font-mono">
                          <div>
                            <span className="text-slate-200 block font-bold">{agent.agentName}</span>
                            <span className="text-[10px] text-slate-400 block">
                              E-Money Pool: {(agent.eMoneyFloat / 1000).toFixed(0)}k BDT | Cash Pool: {(agent.physicalCash / 1000).toFixed(0)}k BDT
                            </span>
                          </div>
                          <div className="flex items-center gap-3">
                            <span className={`text-[10px] font-bold px-2 py-0.5 rounded ${agent.status === 'Optimal' ? 'bg-emerald-950 text-emerald-400 border border-emerald-500/20' : 'bg-rose-950 text-rose-400 border border-rose-500/20'}`}>
                              {agent.status}
                            </span>
                            <button
                              onClick={() => triggerAgentRefloat(agent.id)}
                              className="bg-[#E2125B] hover:bg-[#c10e4e] text-white font-mono text-[9px] font-bold px-2.5 py-1.5 rounded-lg transition-all"
                            >
                              REFRESH FLOAT
                            </button>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>

                </div>

              </div>
            </div>
          </div>
        )}

        {/* TAB 3: DOCUMENT PREDICTOR AND REGRESSIVE KPI HUB */}
        {activeTab === 'predictiveKpi' && (
          <div className="xl:col-span-12 space-y-6">
            <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">
              
              {/* FILE UPLOADER & CALIBRATION MARKER INPUTS */}
              <div className="lg:col-span-5 space-y-6">
                
                {/* ADVANCED MULTI-FORMAT UPLOAD HUB */}
                <div 
                  className={`bg-slate-900/40 border-2 rounded-3xl p-6 transition-all ${
                    isDragging ? 'border-[#E2125B] bg-[#E2125B]/5' : 'border-dashed border-slate-800/80 hover:border-slate-700'
                  }`}
                  onDragOver={handleDragOver}
                  onDragLeave={handleDragLeave}
                  onDrop={handleDrop}
                >
                  <h3 className="text-sm font-bold text-slate-100 font-mono mb-2 flex items-center gap-2">
                    📁 Automated Ingestion & Model Recalibration
                  </h3>
                  <p className="text-[11px] text-slate-400 font-mono leading-relaxed mb-4">
                    Drag and drop operational datasets (<strong>CSV, XLS, XLSX, or PDF</strong>) below. The S-FIS regression engine will parse targets, transaction histories, and risk parameters to recalibrate live forecasts.
                  </p>

                  <div className="bg-slate-950/70 border border-slate-800 rounded-2xl p-6 flex flex-col justify-center items-center text-center">
                    <Upload className="w-10 h-10 text-slate-400 mb-3 animate-bounce" />
                    <p className="text-xs font-mono text-slate-300 font-bold">Drag operational datasets here</p>
                    <p className="text-[10px] font-mono text-slate-500 mt-1">or select file from local directory</p>
                    
                    <input 
                      type="file" 
                      ref={fileInputRef}
                      onChange={handleFileUpload}
                      className="hidden" 
                      accept=".csv,.txt,.pdf,.xlsx,.xls"
                      multiple
                    />
                    
                    <button 
                      onClick={() => fileInputRef.current?.click()}
                      className="mt-4 bg-slate-900 border border-slate-800 hover:bg-slate-800 text-slate-200 text-xs font-mono font-bold px-4 py-2 rounded-xl transition-all"
                    >
                      Browse Storage
                    </button>
                  </div>

                  <div className="mt-4 flex justify-between items-center">
                    <button 
                      onClick={downloadSampleTemplate}
                      className="text-[10px] text-slate-400 hover:text-white font-mono flex items-center gap-1.5 transition-all"
                    >
                      <Download className="w-4 h-4 text-[#E2125B]" /> Download Sample Dataset Template
                    </button>
                  </div>
                </div>

                {/* MANUAL PERFORMANCE NODE CALIBRATOR */}
                <div className="bg-slate-900/40 border border-slate-800 p-5 rounded-3xl">
                  <h3 className="text-sm font-bold text-slate-100 font-mono mb-4 flex items-center gap-2">
                    💾 Inject Manual Historical Calibration Record
                  </h3>

                  <form onSubmit={handleInjectManualPerformance} className="space-y-4">
                    <div className="grid grid-cols-2 gap-3">
                      <div>
                        <label className="block text-[9px] uppercase font-mono text-slate-500 font-bold mb-1">Target Limit (BDT)</label>
                        <input 
                          type="number"
                          name="manualTarget"
                          placeholder="e.g. 4000000"
                          className="w-full bg-slate-950 border border-slate-800 text-slate-200 rounded-xl px-3 py-2.5 text-xs font-mono focus:outline-none focus:border-[#E2125B]"
                        />
                      </div>
                      <div>
                        <label className="block text-[9px] uppercase font-mono text-slate-500 font-bold mb-1">Actual Met (BDT)</label>
                        <input 
                          type="number"
                          name="manualActual"
                          placeholder="e.g. 4200000"
                          className="w-full bg-slate-950 border border-slate-800 text-slate-200 rounded-xl px-3 py-2.5 text-xs font-mono focus:outline-none focus:border-[#E2125B]"
                        />
                      </div>
                    </div>

                    <div>
                      <label className="block text-[9px] uppercase font-mono text-slate-500 font-bold mb-1">Performance Reasons & Drivers</label>
                      <textarea 
                        name="manualReason"
                        placeholder="e.g. Inbound wholesale salary payouts, trade flow bottlenecks, weather anomalies..."
                        className="w-full bg-slate-950 border border-slate-800 text-slate-200 rounded-xl px-3 py-2 text-xs font-mono focus:outline-none focus:border-[#E2125B] h-16 resize-none"
                      />
                    </div>

                    <button 
                      type="submit"
                      className="w-full bg-[#E2125B] hover:bg-[#c10e4e] text-white font-mono text-xs font-bold py-3 rounded-xl transition-all shadow-md"
                    >
                      COMMIT CALIBRATION NODE
                    </button>
                  </form>
                </div>

              </div>

              {/* DYNAMIC REGRESSION FORECAST MODEL & HISTORY TRACKER */}
              <div className="lg:col-span-7 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md space-y-6">
                
                <div className="flex flex-col md:flex-row justify-between items-start md:items-center gap-2 pb-4 border-b border-slate-800/80">
                  <div>
                    <h3 className="text-base font-bold text-slate-100 font-mono">
                      Dynamic Forecasting Model: <span className="text-[#E2125B]">{selectedDivision}</span>
                    </h3>
                    <p className="text-[10px] text-slate-400 font-mono mt-0.5">Calculated based on parsed datasets & historical anchors</p>
                  </div>
                  
                  <div className="flex items-center gap-2">
                    <span className="text-[9px] font-mono text-[#E2125B] bg-[#E2125B]/10 px-2 py-0.5 rounded border border-[#E2125B]/20">
                      Predictive Horizon: {predictiveHorizonDays} Days
                    </span>
                  </div>
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                  {/* Historical Injected Timeline */}
                  <div className="bg-slate-950/70 border border-slate-800 p-4 rounded-2xl space-y-3">
                    <span className="text-[10px] font-mono text-slate-500 font-bold uppercase block">Ingested Historical Milestones</span>
                    <div className="space-y-2 max-h-[220px] overflow-y-auto pr-1">
                      {predictiveKpiCalculations.historical.map((hist, idx) => (
                        <div key={idx} className="p-2.5 bg-slate-900/40 border border-slate-800/40 rounded-xl flex justify-between items-center text-[11px] font-mono">
                          <div>
                            <span className="text-slate-200 block font-bold">{hist.day}</span>
                            <p className="text-[9px] text-slate-400 leading-tight mt-0.5 max-w-[170px] line-clamp-1">{hist.primaryReason}</p>
                          </div>
                          <div className="text-right">
                            <span className="text-[#E2125B] font-bold block">{(hist.actual / 100000).toFixed(1)}L BDT</span>
                            <span className="text-[10px] text-emerald-400 font-bold block">{hist.achieveRate}% Met</span>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>

                  {/* Future Forecast Targets */}
                  <div className="bg-slate-950/70 border border-slate-800 p-4 rounded-2xl space-y-3">
                    <span className="text-[10px] font-mono text-slate-500 font-bold uppercase block">Inferred Target Estimates</span>
                    <div className="space-y-2 max-h-[220px] overflow-y-auto pr-1">
                      {predictiveKpiCalculations.forecasts.map((f, idx) => (
                        <div key={idx} className="p-2.5 bg-slate-900/60 border border-slate-800/60 rounded-xl flex justify-between items-center text-[11px] font-mono">
                          <div>
                            <span className="text-[#E2125B] block font-bold">{f.day}</span>
                            <p className="text-[9px] text-slate-400 leading-tight mt-0.5 max-w-[170px] line-clamp-1">{f.trendReason}</p>
                          </div>
                          <div className="text-right">
                            <span className="text-slate-200 font-bold block">{(f.predictedActual / 100000).toFixed(1)}L BDT</span>
                            <span className="text-[10px] text-amber-400 font-bold block">{f.expectedAchieveRate}% Rate</span>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>
                </div>

                {/* LOGS OF PARSED FILES */}
                <div className="pt-4 border-t border-slate-800">
                  <span className="text-[10px] font-mono text-slate-500 font-bold uppercase block mb-3">Operational Document Pipeline Log</span>
                  <div className="space-y-2">
                    {uploadedFiles.map((file, i) => (
                      <div key={i} className="p-2.5 bg-slate-950 border border-slate-800/80 rounded-xl flex justify-between items-center text-[11px] font-mono">
                        <div className="flex items-center gap-2">
                          <CheckCircle className="w-5 h-5 text-emerald-400" />
                          <div>
                            <span className="text-slate-200 block font-bold">{file.name}</span>
                            <span className="text-[9px] text-slate-500 block">Parsed at {file.timestamp}</span>
                          </div>
                        </div>
                        <div className="text-right text-[10px]">
                          <span className="text-[#E2125B] font-bold block">{file.type} Dataset</span>
                          <span className="text-slate-500 block">{file.size}</span>
                        </div>
                      </div>
                    ))}
                    {uploadedFiles.length === 0 && (
                      <div className="p-4 bg-slate-950/40 border border-slate-800/40 rounded-xl text-center text-xs text-slate-500 font-mono">
                        No custom spreadsheet databases currently uploaded. Standard regression defaults are active.
                      </div>
                    )}
                  </div>
                </div>

              </div>

            </div>
          </div>
        )}

        {/* TAB 4: WORKSPACE BIOMETRIC PROFILE INSPECTION */}
        {activeTab === 'profiles' && (
          <div className="xl:col-span-12 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md">
            <h2 className="text-base font-extrabold text-white mb-2 font-mono flex items-center gap-2">
              👤 Hardware & Biometric Profiler Workspace (`transaction_analysis_profile`)
            </h2>
            <p className="text-xs text-slate-400 max-w-3xl font-mono leading-relaxed mb-6">
              Investigate sandbox hardware anomalies, emulator configurations, and spatial telecom networks linked directly to incoming packet flows inside {selectedDivision}.
            </p>

            <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">
              <div className="lg:col-span-4 bg-slate-950/40 p-4 rounded-2xl border border-slate-800 space-y-2 max-h-[480px] overflow-y-auto">
                <span className="text-[10px] text-slate-500 uppercase tracking-widest font-mono font-bold block mb-2">Select Active Stream Packet</span>
                {transactions.map(tx => (
                  <button
                    key={tx.id}
                    onClick={() => setSelectedTxIdForAnalysis(tx.id)}
                    className={`w-full p-3 rounded-xl border text-left transition-all flex justify-between items-center ${
                      selectedTxIdForAnalysis === tx.id 
                        ? 'bg-[#E2125B]/15 border-[#E2125B]' 
                        : 'bg-slate-950 hover:bg-slate-900 border-slate-800/80'
                    }`}
                  >
                    <div>
                      <span className="text-xs font-bold font-mono text-slate-200 block">{tx.id}</span>
                      <span className="text-[10px] text-slate-400 font-mono max-w-[200px] truncate block">{tx.name}</span>
                    </div>
                    <span className={`w-2.5 h-2.5 rounded-full ${tx.score >= 750 ? 'bg-emerald-400' : tx.score >= aiConfidenceCutoff ? 'bg-amber-400' : 'bg-[#E2125B] animate-pulse'}`}></span>
                  </button>
                ))}
              </div>

              <div className="lg:col-span-8 bg-slate-950/70 border border-slate-800 rounded-3xl p-5 relative">
                <div className="flex justify-between items-center mb-6 pb-4 border-b border-slate-800">
                  <h3 className="text-base font-bold text-slate-200 font-mono">
                    Hardware Signature: <span className="text-[#E2125B]">{activeAnalyzedTx?.id}</span>
                  </h3>
                  <span className="text-[9px] font-mono text-slate-500">BIOMETRIC HANDSHAKE TELEMETRY</span>
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                  <div className="space-y-4">
                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">Biometric Profile Speed</span>
                      <div className="flex items-center gap-2 mt-1">
                        <span className={`w-2.5 h-2.5 rounded-full ${activeAnalyzedTx?.imsiVelocity?.includes('High') ? 'bg-[#E2125B] animate-ping' : 'bg-emerald-400'}`}></span>
                        <span className="text-xs text-slate-200 font-mono font-bold">{activeAnalyzedTx?.imsiVelocity}</span>
                      </div>
                    </div>

                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">Terminal IP Signature</span>
                      <p className="text-xs text-rose-400 font-mono mt-0.5">{activeAnalyzedTx?.ip}</p>
                    </div>

                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">Device Sandbox Configuration</span>
                      <p className="text-xs text-slate-200 font-mono mt-0.5">{activeAnalyzedTx?.device}</p>
                    </div>
                  </div>

                  <div className="space-y-4">
                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">Telecom Base Station Area</span>
                      <p className="text-xs text-slate-200 font-mono mt-0.5">{activeAnalyzedTx?.district} ({activeAnalyzedTx?.networkTower})</p>
                    </div>

                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">Target Dynamic Route</span>
                      <p className="text-xs text-slate-200 font-mono mt-0.5">{activeAnalyzedTx?.route}</p>
                    </div>

                    <div>
                      <span className="text-[10px] uppercase font-mono text-slate-500 block font-bold">S-FIS Evaluation Log</span>
                      <p className="text-xs text-slate-300 font-mono bg-slate-900/60 p-3 rounded-xl border border-slate-800/80 mt-1.5 leading-relaxed">
                        {activeAnalyzedTx?.reason}
                      </p>
                    </div>
                  </div>
                </div>

                {/* Bio Signature visual waves */}
                <div className="mt-8 bg-slate-900/40 border border-slate-800/60 p-4 rounded-2xl">
                  <span className="text-[9px] uppercase font-mono text-rose-500 block mb-2 font-bold">Biometric Inbound Authentication Wave Pattern</span>
                  <div className="h-16 flex items-end gap-1 border-b border-slate-800 pb-1">
                    <div className="bg-rose-500/20 w-full h-[25%] rounded-sm"></div>
                    <div className="bg-rose-500/35 w-full h-[55%] rounded-sm"></div>
                    <div className="bg-rose-500/50 w-full h-[70%] rounded-sm"></div>
                    <div className="bg-[#E2125B] w-full h-[95%] rounded-sm animate-pulse"></div>
                    <div className="bg-rose-500/60 w-full h-[45%] rounded-sm"></div>
                    <div className="bg-rose-500/20 w-full h-[15%] rounded-sm"></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* TAB 5: AUTOMATED FRAUD RULES SECURITY */}
        {activeTab === 'rules' && (
          <div className="xl:col-span-12 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md">
            <h2 className="text-base font-extrabold text-white mb-2 font-mono flex items-center gap-2">
              ⚙️ Automated Fraud Overrides & Threshold Limits (`fraud_rules_automation`)
            </h2>
            <p className="text-xs text-slate-400 max-w-3xl font-mono leading-relaxed mb-6">
              Configure parameters to instantly toggle standard speed gate fast-tracks off, redirecting suspect district pools to the secure suspense escrow system.
            </p>

            <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
              <div className="bg-slate-950/80 border border-slate-800 p-5 rounded-2xl">
                <span className="text-[9px] text-[#E2125B] uppercase tracking-widest font-mono font-bold block mb-1">VELOCITY ACCELERATION BDT OVERRIDE</span>
                <h3 className="text-xs font-bold font-mono text-slate-200 mb-4">Single Transaction Hard Limit</h3>
                <input 
                  type="range" 
                  min="20000" 
                  max="300000" 
                  step="5000"
                  value={ruleVelocityThreshold} 
                  onChange={(e) => {
                    setRuleVelocityThreshold(parseInt(e.target.value, 10));
                    showToast(`Security threshold shifted to ${parseInt(e.target.value, 10).toLocaleString()} BDT`, 'success');
                  }}
                  className="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-[#E2125B] mb-4"
                />
                <div className="flex justify-between text-[11px] font-mono text-slate-400">
                  <span>Current Limit:</span>
                  <strong className="text-white">{ruleVelocityThreshold.toLocaleString()} BDT</strong>
                </div>
              </div>

              <div className="bg-slate-950/80 border border-slate-800 p-5 rounded-2xl">
                <span className="text-[9px] text-[#E2125B] uppercase tracking-widest font-mono font-bold block mb-1">AI DECISION THRESHOLD CUTOFF</span>
                <h3 className="text-xs font-bold font-mono text-slate-200 mb-4">Confidence Level Parameters</h3>
                <input 
                  type="range" 
                  min="200" 
                  max="800" 
                  step="25"
                  value={aiConfidenceCutoff} 
                  onChange={(e) => {
                    setAiConfidenceCutoff(parseInt(e.target.value, 10));
                    showToast(`Confidence scale updated to ${parseInt(e.target.value, 10)} / 1000`, 'success');
                  }}
                  className="w-full h-1 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-[#E2125B] mb-4"
                />
                <div className="flex justify-between text-[11px] font-mono text-slate-400">
                  <span>Current Cutoff:</span>
                  <strong className="text-white">{aiConfidenceCutoff} / 1000</strong>
                </div>
              </div>

              <div className="bg-slate-950/80 border border-slate-800 p-5 rounded-2xl flex flex-col justify-between">
                <div>
                  <span className="text-[9px] text-[#E2125B] uppercase tracking-widest font-mono font-bold block mb-1">HARDWARE THREAT PROTOCOL</span>
                  <h3 className="text-xs font-bold font-mono text-slate-200 mb-3">Violations Action Mode</h3>
                </div>

                <div className="grid grid-cols-2 gap-3">
                  <button 
                    type="button"
                    onClick={() => { setRuleHighRiskIMSIAction('Hold'); showToast('Action updated: Route to Suspense Escrow Pool', 'info'); }}
                    className={`p-3 rounded-xl border text-[10px] font-mono font-bold text-center transition-all ${
                      ruleHighRiskIMSIAction === 'Hold' 
                        ? 'bg-amber-950/20 text-amber-400 border-amber-500/50' 
                        : 'bg-slate-950 border-slate-800 text-slate-500'
                    }`}
                  >
                    🟡 SUSPENSE HOLD
                  </button>
                  <button 
                    type="button"
                    onClick={() => { setRuleHighRiskIMSIAction('Block'); showToast('Action updated: Direct Hardware Ban', 'error'); }}
                    className={`p-3 rounded-xl border text-[10px] font-mono font-bold text-center transition-all ${
                      ruleHighRiskIMSIAction === 'Block' 
                        ? 'bg-rose-950/20 text-rose-500 border-rose-500/50' 
                        : 'bg-slate-950 border-slate-800 text-slate-500'
                    }`}
                  >
                    🔴 HARD BLOCK
                  </button>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* TAB 6: SUSPENSE ESCROW VAULT QUARANTINE */}
        {activeTab === 'vault' && (
          <div className="xl:col-span-12 bg-slate-900/40 border border-slate-800/80 p-5 rounded-3xl backdrop-blur-md">
            <h2 className="text-base font-extrabold text-white mb-2 font-mono flex items-center gap-2">
              🔒 S-FIS Temporary Isolation Vault Hold (`suspense_account_investigation`)
            </h2>
            <p className="text-xs text-slate-400 max-w-3xl font-mono leading-relaxed mb-6">
              This sandbox holds transactions violating baseline velocity and hardware profiling limits, preventing immediate settlement of funds until a manual bypass or OTP code override is executed.
            </p>

            <div className="space-y-4">
              {transactions.filter(t => t.status === 'In Suspense Account' || t.status === 'Pending OTP Validation').map(tx => (
                <div key={tx.id} className="bg-slate-950/80 border border-slate-800 p-4 rounded-2xl flex flex-col lg:flex-row justify-between items-start lg:items-center gap-4 transition-all">
                  <div className="space-y-1 font-mono">
                    <div className="flex items-center gap-2">
                      <span className="text-xs font-black text-slate-200">{tx.id}</span>
                      <span className="bg-amber-500/15 text-amber-400 border border-amber-500/25 text-[9px] font-bold px-2 py-0.5 rounded-full uppercase tracking-wider">
                        TEMPORARY SECURITY BLOCK
                      </span>
                      <span className="text-[10px] text-slate-500">{tx.district}, {tx.division}</span>
                    </div>

                    <p className="text-xs font-bold text-slate-300">Target wallet owner: {tx.name}</p>
                    <p className="text-xs text-[#E2125B] font-extrabold">Quarantined Amount: {tx.amount.toLocaleString()} BDT</p>
                    <p className="text-[10px] text-slate-400 leading-relaxed mt-1">
                      Reason: <strong className="text-slate-300">{tx.reason}</strong>
                    </p>
                  </div>

                  <div className="flex flex-wrap items-center gap-3 w-full lg:w-auto font-mono">
                    <input 
                      type="password" 
                      placeholder="Enter Bypass OTP Code"
                      value={otpInputs[tx.id] || ''}
                      onChange={(e) => setOtpInputs({ ...otpInputs, [tx.id]: e.target.value })}
                      className="bg-slate-900 border border-slate-800 rounded-xl px-3 py-2 text-xs text-white max-w-[160px] focus:outline-none focus:border-[#E2125B]"
                    />

                    <button 
                      onClick={() => {
                        if (!otpInputs[tx.id]) {
                          showToast('Please specify a bypass OTP value.', 'error');
                          return;
                        }
                        resolveSuspense(tx.id, true);
                      }}
                      className="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-bold px-4 py-2 rounded-xl transition-all"
                    >
                      CLEAR GREEN LANE
                    </button>

                    <button 
                      onClick={() => resolveSuspense(tx.id, false)}
                      className="bg-rose-600 hover:bg-rose-700 text-white text-xs font-bold px-4 py-2 rounded-xl transition-all"
                    >
                      FREEZE & BLACKLIST
                    </button>
                  </div>
                </div>
              ))}

              {transactions.filter(t => t.status === 'In Suspense Account' || t.status === 'Pending OTP Validation').length === 0 && (
                <div className="text-center py-16">
                  <p className="text-xs text-slate-500 font-mono">All suspicious escrow holds inside {selectedDivision} have been fully evaluated and cleared.</p>
                </div>
              )}
            </div>
          </div>
        )}
      </div>

      {/* CORE BRANDING SYSTEM SUMMARY */}
      <footer className="mt-8 border-t border-slate-900 pt-6">
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 text-[10px] text-slate-500 font-mono leading-relaxed">
          <div>
            <h5 className="font-extrabold text-slate-300 uppercase mb-1">bKash-SMOTE Inbound Oversampler</h5>
            <p>
              To eliminate extreme class imbalances, the oversampling algorithms dynamically construct synthetic transaction profiles to preserve balanced risk indexes.
            </p>
          </div>
          <div>
            <h5 className="font-extrabold text-slate-300 uppercase mb-1">MFS Automated Speed Gate (XGBoost)</h5>
            <p>
              Filters transactions at less than ~7.1ms latency, checking terminal structures and geographical base-station hops.
            </p>
          </div>
          <div>
            <h5 className="font-extrabold text-slate-300 uppercase mb-1">Predictive Escrow Suspense Loops (ANN)</h5>
            <p>
              Anomalous transactions are isolated into temporary escrow vaults, requiring validation of dynamic security rules.
            </p>
          </div>
        </div>

        <div className="text-center text-[9px] text-slate-600 mt-8 border-t border-slate-950 pt-4 font-mono">
          bKash Sentinel S-FIS Command Suite. Strictly internal use only.
        </div>
      </footer>
    </div>
  );
}
