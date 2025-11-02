<template>
  <section class="projects reveal">
  <h2 class="section-heading">Projects</h2>
    <div class="section-shell more-defined">
      <div class="proj-list">
  <div v-for="(p, idx) in items" :key="idx" :id="p.id ? p.id : undefined" class="proj-item tile reveal" :class="{ 'delay-1': idx===0, 'delay-2': idx===1 }">
          <div class="timeline-head" role="button" tabindex="0" @click="toggle(idx)" @keyup.enter="toggle(idx)" :aria-expanded="p.open">
            <div class="head-content">
              <h3>{{ p.title }}</h3>
              <p class="muted">{{ p.tech }}</p>
            </div>
          </div>


          <transition name="fade-slide">
            <div v-show="p.open" class="timeline-body">
              <div class="expanded-shell">
                <div class="vertical-timeline">
                  <div v-for="(e, ei) in p.entries" :key="ei" class="vt-entry">
                    <div class="vt-content">
                      <h4>{{ e.title }}</h4>
                          <ul>
                            <li v-for="(d, di) in e.details" :key="di">
                              <template v-if="typeof d === 'string'">
                                {{ d }}
                              </template>
                              <template v-else-if="d.type === 'link'">
                                <a :href="d.href" target="_blank" rel="noopener">{{ d.text }}</a>
                              </template>
                              <template v-else-if="d.type === 'text'">
                                {{ d.text }}
                              </template>
                              <template v-else>
                                {{ d }}
                              </template>
                            </li>
                          </ul>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </div>
  </section>
</template>


<script setup lang="ts">
import { reactive } from 'vue'


const items = reactive([
  {
    id: 'loca',
    title: 'Loca (BetterMaps) — AI-Powered Navigation & Route Planning',
    tech: 'Python (Flask), Vue.js 3, Google Maps APIs, Gemini AI, Docker',
    open: false,
    entries: [
      { 
        title: 'Overview', 
        details: [
          'An intelligent routing system that combines natural language processing with real-time geospatial data to plan optimized routes with personalized POI stops.',
          'Users can request routes in natural language (Romanian/English) like "Take me from Bucharest to Sibiu with an OMV stop and restaurant in Sinaia".',
          'AI-powered orchestrator uses Gemini to analyze requests, plan API call sequences, score waypoint options, and provide reasoning for selections.',
          'Real-time map visualization with interactive markers, route details (distance, duration), and stop explanations.'
        ] 
      },
      { 
        title: 'Architecture & Data Flow', 
        details: [
          'Frontend (Vue.js + Vite) sends natural language route requests to Flask backend on port 5000.',
          'Backend smart orchestrator receives request and delegates to Gemini AI for intelligent API orchestration.',
          'Gemini AI analyzes request and plans sequence of Google Maps API calls: geocoding for city/location coordinates, directions for route planning, places search for POI discovery.',
          'AI scores and ranks POIs based on proximity to route, user preferences, and contextual relevance (e.g., prioritizing stops in specific cities).',
          'Backend converts structured response to frontend-compatible format with coordinates, markers, and route geometry.',
          'Frontend MapView component renders route on Google Maps with custom markers, polylines, and interactive info windows.'
        ] 
      },
      { 
        title: 'Key Features', 
        details: [
          'Natural language input processing in Romanian and English with context understanding.',
          'Smart POI detection: finds specific brands (OMV, Petrom), restaurant types, and services along route or in designated cities.',
          'Multi-strategy fallback logic for reliable POI discovery when primary searches fail.',
          'GPS-precise coordinates for navigation integration, not just city-level approximations.',
          'AI reasoning transparency: system explains why each stop was selected with scoring details.',
          'Secure dashboard with Google OAuth2 authentication and user session management.',
          'Responsive design with mobile-first approach for on-the-go route planning.'
        ] 
      },
      { 
        title: 'Technical Implementation', 
        details: [
          'Backend: Flask REST API with Python 3.8+, environment-based configuration for API keys, CORS handling for frontend communication.',
          'AI Orchestration: smart_orchestrator.py coordinates Gemini API calls with custom prompts from prompts/ directory for route planning logic.',
          'Google Maps Integration: geocoding API for address/city to coordinates, directions API for route calculation, places API for POI search with radius and type filters.',
          'Frontend: Vue.js 3 Composition API with Axios for HTTP requests, Google Maps JavaScript API integration in MapView component.',
          'Development Setup: Docker containers for reproducible environment, separate terminal instances for frontend (port 5173) and backend (port 5000).'
        ] 
      },
      { 
        title: 'What I Learned', 
        details: [
          'Integrating multiple Google APIs (Maps, Gemini) in a cohesive system with proper error handling and fallback strategies.',
          'Building AI orchestrators that intelligently sequence API calls based on natural language understanding.',
          'Managing API rate limits, quotas, and authentication for production-ready geospatial applications.',
          'Frontend-backend separation with RESTful API design and real-time data synchronization.',
          'Handling multilingual input and Romanian-specific context in AI prompts.'
        ] 
      },
      { 
        title: 'Next Steps', 
        details: [
          'Add persistent route history and favorite locations with user profile integration.',
          'Implement real-time traffic data and dynamic rerouting suggestions.',
          'Expand POI categories and preference learning based on user selection patterns.',
          'Mobile app development (Flutter/React Native) for native navigation experience.',
          'Integrate voice input for hands-free route requests while driving.'
        ] 
      },
    ],
  },
  {
    id: 'receipt',
    title: 'Re-ceipt — AI-Powered Receipt Management & Expense Tracking',
    tech: 'Spring Boot 3.5, Vue.js 3, Python, Apache Kafka, EasyOCR, Ollama LLM',
    open: false,
    entries: [
      { 
        title: 'Overview', 
        details: [
          'A modern expense management platform that automates receipt processing through computer vision (OCR) and large language models for structured data extraction.',
          'Users upload receipt images via drag-and-drop interface; system automatically extracts text, corrects errors, categorizes products, and tracks spending.',
          'Event-driven architecture using Kafka enables async processing pipeline from upload to analytics without blocking user experience.',
          'Provides spending analytics dashboard with categorized expenses, balance overview, and receipt history management.'
        ] 
      },
      { 
        title: 'Data Flow & Processing Pipeline', 
        details: [
          'Step 1 — Upload: User uploads receipt image via Vue.js frontend with drag-and-drop or file selector.',
          'Step 2 — API Ingestion: Frontend sends multipart/form-data to Spring Boot REST endpoint; backend validates image format, size, and stores metadata.',
          'Step 3 — Event Publishing: Spring Kafka producer publishes receipt-upload event to Kafka topic with receipt ID and storage path.',
          'Step 4 — OCR Processing: Python consumer with EasyOCR reads Kafka message, loads image from storage, extracts text using deep learning models.',
          'Step 5 — LLM Correction: Extracted text sent to Ollama LLM (local inference) to correct OCR errors, format Romanian currency, structure into JSON schema.',
          'Step 6 — Categorization: Second Python service consumes corrected data, uses Ollama to categorize products (groceries, electronics, fuel, etc.) based on merchant and items.',
          'Step 7 — Analytics Update: Processed data published back to Kafka for real-time dashboard updates; frontend displays spending breakdown and receipt details.'
        ] 
      },
      { 
        title: 'Key Features', 
        details: [
          'Automated OCR with EasyOCR: high-accuracy text extraction from receipt images with support for rotated/skewed images and multiple languages.',
          'LLM-powered error correction: Ollama models fix common OCR mistakes (0 vs O, l vs 1), handle Romanian diacritics, and structure data into consistent JSON format.',
          'Smart product categorization: AI analyzes merchant names and product descriptions to auto-categorize expenses for budgeting.',
          'Real-time processing: Kafka event streaming enables non-blocking async pipeline; users see upload confirmation immediately while processing happens in background.',
          'Expense analytics dashboard: visualizations for spending by category, monthly trends, top merchants, and balance tracking.',
          'Receipt history management: searchable archive with filters by date, category, merchant, and amount range.',
          'Configurable settings: user preferences for categories, budget limits, and notification thresholds.'
        ] 
      },
      { 
        title: 'Technical Architecture', 
        details: [
          'Frontend: Vue.js 3 Composition API with Vite build tooling, Axios for API communication, responsive CSS with mobile-first design for receipt capture on phones.',
          'Backend: Spring Boot 3.5.6 (Java 21) with Spring Web for REST endpoints, Spring Kafka for event streaming, multipart file handling with size/type validation.',
          'Message Broker: Apache Kafka with Zookeeper coordination, Confluent distribution running in Docker, receipt-upload topic for OCR queue, receipt-processed topic for analytics.',
          'OCR Service: Python 3.8+ with EasyOCR library for text extraction, OpenCV for image preprocessing (contrast, rotation, noise reduction), Kafka consumer/producer for event handling.',
          'LLM Service: Ollama for local LLM inference (privacy-friendly, no external API costs), custom prompts for Romanian receipt format and error correction patterns.',
          'Infrastructure: Docker Compose orchestrates Kafka, Zookeeper, Python services; environment-based config for Kafka brokers and service endpoints.'
        ] 
      },
      { 
        title: 'What I Learned', 
        details: [
          'Building event-driven microservices with Kafka for async processing and system decoupling.',
          'Integrating computer vision (OCR) with LLMs to improve accuracy through multi-stage processing pipelines.',
          'Handling file uploads, storage, and retrieval in distributed systems with proper error handling.',
          'Spring Boot Kafka integration: producer/consumer configuration, serialization, offset management, and error recovery.',
          'Local LLM deployment with Ollama: prompt engineering for structured output, managing inference performance and resource usage.',
          'Designing RESTful APIs for file upload with progress tracking and async status updates.'
        ] 
      },
      { 
        title: 'Next Steps', 
        details: [
          'Mobile app development with Flutter for native camera integration and instant receipt capture.',
          'Add receipt duplicate detection using image similarity algorithms to prevent double entries.',
          'Implement subscription tracking: identify recurring expenses and alert users to upcoming renewals.',
          'Multi-currency support with automatic exchange rate conversion for international receipts.',
          'Export functionality: generate CSV/Excel reports for accounting software integration (QuickBooks, Xero).',
          'Collaborative features: shared household accounts with multi-user access and expense splitting.',
          'Advanced analytics: spending predictions, budget recommendations, and anomaly detection for unusual transactions.'
        ] 
      },
    ],
  },
  {
    id: 'smart-home',
    title: 'Smart Autonomous Home',
    tech: 'STM32, Raspberry Pi, IoT, Embedded C',
    open: false,
    entries: [
      { title: 'Overview', details: [
        'A modular IoT-driven smart home prototype focused on autonomy, energy efficiency and local decision-making.',
        'Combines embedded controllers (STM32) with a Raspberry Pi gateway and a Spring Boot backend for telemetry, control and user dashboards.',
        { type: 'link', text: 'Smart Autonomous Home post', href: 'https://www.linkedin.com/pulse/building-smart-autonomous-passive-house-victor-florescu-dkcrf/?trackingId=QWzY9mOoRSm3gTK5K903UA%3D%3D' },
      ] },
      { title: 'Design timeline', details: [
        'Phase 1 — Concept & requirements: system-level specs, security and power budgets.',
        'Phase 2 — Hardware selection & prototyping: select STM32 families, prototype PCBs, and sensors.',
        'Phase 3 — Firmware & comms: implement DMA/USART patterns, robust serial protocol and object-pool memory model.',
        'Phase 4 — Gateway & dashboard: Pi gateway, Spring backend, and Vue.js dashboard integration.',
        'Phase 5 — Integration & field testing: power-management tuning, CV and access control validation.',
        'Phase 6 — Optimization & scaling: CAN/ROS2 planning, predictive models and deployment readiness.'
      ] },
      { title: 'Key Features', details: [
        'Local decision node using Raspberry Pi for real-time control and a web dashboard in Vue.js.',
        'Secure Google OAuth2 login for the dashboard and role-based access to controls and logs.',
        'Custom C serial library (struct-based) to handle JSON-over-serial between STM32 and Raspberry Pi.',
        'Computer vision for license plate recognition (YOLO fine-tuned on Romanian plates) and RFID-based door access.'
      ] },
      { title: 'Hardware & Software', details: [
        'STM32 NUCLEO boards for sensor reading, DMA-based USART with interrupt-driven non-blocking comms.',
        'Raspberry Pi as gateway and dashboard host; Spring Boot backend (Dockerized) with MySQL and SSE/WebSocket components.',
        'Actuators: servos, relays, neopixel LEDs, solenoids; power-management with MOSFET switching (fallback to off-the-shelf module).' 
      ] },
      { title: 'What I learned', details: [
        'Practical embedded patterns: DMA, interrupts, and modular firmware design.',
        'System design and scaling: from single-board proof-of-concept to distributed STM32 network + CAN/ROS2 ideas.',
        'Security and infra: OAuth2 integration, observability plans (logs, metrics, traces), and moving from self-signed to DV TLS.'
      ] },
      { title: 'Next steps', details: [
        'Replace dynamic allocation in serial library with object pools; integrate Kafka for scalable service communication.',
        'Move toward distributed STM32 nodes with CAN bus and potentially migrate Pi components to ROS2 nodes.',
        'Add prediction models for autonomous optimization (anticipate window openings, occupancy, and energy use).'
      ] },
    ],
  },
])


function toggle(i: number) {
  if (!items || !items[i]) return
  items[i].open = !items[i].open
}


// auto-expand when hash matches a project's id
import { onMounted } from 'vue'
import { useRoute } from 'vue-router'
const route = useRoute()
onMounted(() => {
  if (route.hash) {
    const target = route.hash.replace('#', '')
    const idx = items.findIndex(it => it.id === target)
    if (idx >= 0) {
      // wait a bit so the router finishes scroll, then open
      setTimeout(() => {
        if (items[idx]) items[idx].open = true
      }, 250)
    }
  }
  // If opened via navbar (sessionStorage flag) and no hash navigation happened, open first
  try {
    const nav = sessionStorage.getItem('navEntry')
    if (nav === '/projects' && !route.hash) {
      if (items[0]) items[0].open = true
    }
  } catch (e) {}
  try { sessionStorage.removeItem('navEntry') } catch (e) {}
})
</script>


<style scoped>
.proj-list { display: grid; gap: 0.6rem; grid-template-columns: 1fr }
.proj-item { background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding: 1.2rem; border-radius: 12px; min-height: 120px; display:flex; flex-direction:column; justify-content:center }
.proj-item h3 { margin: 0 0 0.15rem }


.timeline-head { display:flex; gap:1rem; align-items:flex-start; cursor:pointer }
.head-content { flex:1 }
.timeline-body { margin-top:0.8rem }
.expanded-shell { background: linear-gradient(180deg, rgba(0,0,0,0.28), rgba(0,0,0,0.16)); padding:1rem; border-radius:12px; border:1px solid rgba(255,255,255,0.025) }
.vertical-timeline { position:relative; display:flex; flex-direction:column; gap:1rem; padding-left:36px }
.vertical-timeline:before { content:''; position:absolute; left:12px; top:8px; bottom:8px; width:2px; background: linear-gradient(180deg, var(--accent), rgba(255,255,255,0.02)) }
.vt-entry { display:flex; gap:1rem; align-items:flex-start; padding:0.6rem; border-radius:8px; background: rgba(0,0,0,0.06); position:relative }
.vt-entry:before { content:none }
.vt-content { flex:1 }
.vt-content h4 { margin:0 0 0.35rem }
.vt-content ul { margin:0; padding-left:1.05rem }


@media (min-width: 900px) {
  /* keep single-column stack on wide screens to match Education layout */
  .proj-list { grid-template-columns: 1fr }
  .expanded-shell { padding:1.25rem }
  .vertical-timeline { padding-left:56px }
}


/* transition copied from EducationView */
.fade-slide-enter-active, .fade-slide-leave-active { transition: all .28s cubic-bezier(.2,.9,.2,1) }
.fade-slide-enter-from { opacity:0; transform: translateY(-6px) }
.fade-slide-enter-to { opacity:1; transform:none }
.fade-slide-leave-from { opacity:1 }
.fade-slide-leave-to { opacity:0; transform: translateY(-6px) }


</style>
