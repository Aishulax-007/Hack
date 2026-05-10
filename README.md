import { useState, useEffect } from "react";

const COLORS = {
  sand: "#F5F0E8",
  clay: "#C4703A",
  forest: "#2D5016",
  sage: "#7A9E5A",
  sky: "#4A90D9",
  night: "#1A1F2E",
  gold: "#E8A820",
  coral: "#E85D3A",
  cream: "#FDFAF4",
  muted: "#8A8580",
};

const GOOGLE_FONT = `@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=DM+Sans:wght@300;400;500;600&display=swap');`;

const globalStyle = `
${GOOGLE_FONT}
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: 'DM Sans', sans-serif; background: ${COLORS.cream}; color: ${COLORS.night}; }
h1,h2,h3,h4 { font-family: 'Playfair Display', serif; }

.app { min-height: 100vh; display: flex; flex-direction: column; }

/* NAV */
.nav { background: ${COLORS.night}; color: white; padding: 0 32px; height: 60px; display: flex; align-items: center; justify-content: space-between; position: sticky; top: 0; z-index: 100; }
.nav-logo { font-family: 'Playfair Display', serif; font-size: 22px; font-weight: 900; color: ${COLORS.gold}; cursor: pointer; letter-spacing: -0.5px; }
.nav-logo span { color: white; }
.nav-links { display: flex; gap: 8px; }
.nav-btn { background: none; border: none; color: #ccc; padding: 8px 14px; border-radius: 8px; cursor: pointer; font-size: 13px; font-family: 'DM Sans', sans-serif; font-weight: 500; transition: all 0.2s; }
.nav-btn:hover, .nav-btn.active { background: rgba(255,255,255,0.1); color: white; }
.nav-btn.cta { background: ${COLORS.gold}; color: ${COLORS.night}; font-weight: 600; }
.nav-btn.cta:hover { background: #d4940e; }

/* SCREENS */
.screen { flex: 1; }

/* AUTH */
.auth-wrap { min-height: calc(100vh - 60px); display: flex; }
.auth-left { flex: 1; background: linear-gradient(160deg, ${COLORS.forest} 0%, ${COLORS.night} 100%); padding: 60px; display: flex; flex-direction: column; justify-content: center; position: relative; overflow: hidden; }
.auth-left::before { content: '✈'; font-size: 180px; position: absolute; right: -20px; top: 50%; transform: translateY(-50%); opacity: 0.05; }
.auth-left h1 { font-size: 52px; color: white; line-height: 1.1; margin-bottom: 20px; }
.auth-left h1 span { color: ${COLORS.gold}; }
.auth-left p { color: rgba(255,255,255,0.7); font-size: 16px; line-height: 1.7; max-width: 380px; }
.auth-pills { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 32px; }
.auth-pill { background: rgba(255,255,255,0.1); color: rgba(255,255,255,0.8); padding: 8px 16px; border-radius: 20px; font-size: 13px; border: 1px solid rgba(255,255,255,0.15); }
.auth-right { width: 420px; background: white; padding: 60px 48px; display: flex; flex-direction: column; justify-content: center; }
.auth-right h2 { font-size: 30px; margin-bottom: 8px; }
.auth-right .sub { color: ${COLORS.muted}; font-size: 14px; margin-bottom: 36px; }
.auth-tabs { display: flex; background: ${COLORS.sand}; border-radius: 10px; padding: 4px; margin-bottom: 28px; }
.auth-tab { flex: 1; padding: 10px; text-align: center; border-radius: 8px; cursor: pointer; font-size: 14px; font-weight: 500; transition: all 0.2s; }
.auth-tab.active { background: white; color: ${COLORS.night}; box-shadow: 0 2px 8px rgba(0,0,0,0.08); }

/* FORMS */
.field { margin-bottom: 18px; }
.field label { display: block; font-size: 12px; font-weight: 600; color: ${COLORS.muted}; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 6px; }
.field input, .field textarea, .field select { width: 100%; padding: 12px 16px; border: 1.5px solid #E8E4DC; border-radius: 10px; font-size: 14px; font-family: 'DM Sans', sans-serif; outline: none; transition: border 0.2s; background: white; }
.field input:focus, .field textarea:focus, .field select:focus { border-color: ${COLORS.clay}; }
.field textarea { resize: vertical; min-height: 80px; }
.btn { padding: 13px 24px; border-radius: 10px; border: none; cursor: pointer; font-size: 14px; font-weight: 600; font-family: 'DM Sans', sans-serif; transition: all 0.2s; }
.btn-primary { background: ${COLORS.clay}; color: white; }
.btn-primary:hover { background: #b06030; }
.btn-secondary { background: ${COLORS.sand}; color: ${COLORS.night}; }
.btn-secondary:hover { background: #ece7de; }
.btn-full { width: 100%; }
.btn-ghost { background: none; border: 1.5px solid #E8E4DC; color: ${COLORS.night}; }
.btn-ghost:hover { border-color: ${COLORS.clay}; color: ${COLORS.clay}; }
.btn-sm { padding: 8px 16px; font-size: 12px; border-radius: 8px; }
.btn-danger { background: #fee2e2; color: #dc2626; }
.btn-danger:hover { background: #fecaca; }

/* DASHBOARD */
.dash-hero { background: linear-gradient(135deg, ${COLORS.night} 0%, #2d3a5c 100%); color: white; padding: 48px 40px; position: relative; overflow: hidden; }
.dash-hero::after { content: '🌍'; font-size: 140px; position: absolute; right: 40px; top: 50%; transform: translateY(-50%); opacity: 0.15; }
.dash-hero h1 { font-size: 38px; margin-bottom: 8px; }
.dash-hero h1 span { color: ${COLORS.gold}; }
.dash-hero p { opacity: 0.7; font-size: 15px; margin-bottom: 24px; }
.dash-stats { display: flex; gap: 24px; }
.dash-stat { background: rgba(255,255,255,0.1); padding: 16px 24px; border-radius: 12px; }
.dash-stat .num { font-size: 28px; font-weight: 700; font-family: 'Playfair Display', serif; }
.dash-stat .lbl { font-size: 12px; opacity: 0.7; margin-top: 2px; }
.dash-body { padding: 36px 40px; }
.section-title { font-size: 22px; margin-bottom: 20px; }
.section-title span { color: ${COLORS.clay}; }

/* TRIP CARDS */
.trips-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; margin-bottom: 40px; }
.trip-card { background: white; border-radius: 16px; overflow: hidden; box-shadow: 0 2px 12px rgba(0,0,0,0.06); transition: transform 0.2s, box-shadow 0.2s; cursor: pointer; border: 1.5px solid transparent; }
.trip-card:hover { transform: translateY(-4px); box-shadow: 0 8px 24px rgba(0,0,0,0.12); border-color: ${COLORS.clay}; }
.trip-card-banner { height: 120px; display: flex; align-items: center; justify-content: center; font-size: 56px; }
.trip-card-body { padding: 18px; }
.trip-card-body h3 { font-size: 17px; margin-bottom: 4px; }
.trip-meta { display: flex; gap: 12px; margin-top: 10px; }
.trip-meta-item { display: flex; align-items: center; gap: 5px; font-size: 12px; color: ${COLORS.muted}; }
.trip-card-actions { display: flex; gap: 8px; margin-top: 14px; padding-top: 14px; border-top: 1px solid ${COLORS.sand}; }
.badge { display: inline-flex; align-items: center; padding: 4px 10px; border-radius: 20px; font-size: 11px; font-weight: 600; }
.badge-green { background: #dcfce7; color: #16a34a; }
.badge-blue { background: #dbeafe; color: #2563eb; }
.badge-orange { background: #ffedd5; color: #ea580c; }

/* DESTINATIONS */
.dest-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 14px; margin-bottom: 40px; }
.dest-card { background: white; border-radius: 12px; padding: 20px 16px; text-align: center; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.05); transition: all 0.2s; border: 1.5px solid transparent; }
.dest-card:hover { border-color: ${COLORS.sky}; transform: translateY(-2px); }
.dest-card .emoji { font-size: 40px; margin-bottom: 10px; }
.dest-card h4 { font-size: 14px; font-family: 'DM Sans', sans-serif; font-weight: 600; }
.dest-card p { font-size: 11px; color: ${COLORS.muted}; margin-top: 2px; }

/* MODAL */
.modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 200; display: flex; align-items: center; justify-content: center; padding: 20px; }
.modal { background: white; border-radius: 20px; width: 100%; max-width: 520px; max-height: 90vh; overflow-y: auto; box-shadow: 0 20px 60px rgba(0,0,0,0.3); }
.modal-header { padding: 28px 28px 0; display: flex; align-items: flex-start; justify-content: space-between; }
.modal-header h2 { font-size: 24px; }
.modal-close { background: ${COLORS.sand}; border: none; width: 32px; height: 32px; border-radius: 50%; cursor: pointer; font-size: 16px; display: flex; align-items: center; justify-content: center; }
.modal-body { padding: 24px 28px 28px; }

/* ITINERARY */
.itinerary-wrap { display: grid; grid-template-columns: 300px 1fr; min-height: calc(100vh - 60px); }
.itin-sidebar { background: white; border-right: 1px solid #E8E4DC; padding: 28px; overflow-y: auto; }
.itin-sidebar h3 { font-size: 18px; margin-bottom: 20px; }
.stop-item { background: ${COLORS.sand}; border-radius: 12px; padding: 14px; margin-bottom: 12px; cursor: pointer; border: 2px solid transparent; transition: all 0.2s; }
.stop-item.active, .stop-item:hover { border-color: ${COLORS.clay}; background: #fdf4ec; }
.stop-item .city-name { font-weight: 600; font-size: 15px; }
.stop-item .stop-meta { font-size: 12px; color: ${COLORS.muted}; margin-top: 4px; }
.itin-main { padding: 36px; background: ${COLORS.cream}; overflow-y: auto; }
.day-block { background: white; border-radius: 16px; padding: 24px; margin-bottom: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
.day-block .day-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 16px; }
.day-block .day-label { font-size: 13px; color: ${COLORS.muted}; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px; }
.activity-item { display: flex; align-items: center; gap: 14px; padding: 12px 0; border-bottom: 1px solid ${COLORS.sand}; }
.activity-item:last-child { border-bottom: none; }
.activity-icon { width: 40px; height: 40px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 20px; flex-shrink: 0; }
.activity-info { flex: 1; }
.activity-info .name { font-weight: 600; font-size: 14px; }
.activity-info .meta { font-size: 12px; color: ${COLORS.muted}; margin-top: 2px; }
.activity-cost { font-weight: 700; color: ${COLORS.forest}; font-size: 14px; }

/* BUDGET */
.budget-wrap { padding: 36px 40px; }
.budget-cards { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin-bottom: 32px; }
.budget-card { background: white; border-radius: 16px; padding: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
.budget-card .icon { font-size: 28px; margin-bottom: 10px; }
.budget-card .amount { font-size: 24px; font-weight: 700; font-family: 'Playfair Display', serif; }
.budget-card .label { font-size: 12px; color: ${COLORS.muted}; margin-top: 4px; }
.budget-bar-wrap { background: white; border-radius: 16px; padding: 28px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); margin-bottom: 24px; }
.budget-bar-wrap h3 { font-size: 18px; margin-bottom: 20px; }
.budget-bar-row { margin-bottom: 16px; }
.budget-bar-info { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 13px; }
.budget-bar-track { background: ${COLORS.sand}; border-radius: 8px; height: 10px; overflow: hidden; }
.budget-bar-fill { height: 100%; border-radius: 8px; transition: width 1s; }

/* CHECKLIST */
.checklist-wrap { padding: 36px 40px; max-width: 700px; }
.checklist-cats { display: flex; gap: 10px; margin-bottom: 24px; flex-wrap: wrap; }
.cat-btn { padding: 8px 18px; border-radius: 20px; border: 1.5px solid #E8E4DC; background: white; cursor: pointer; font-size: 13px; font-weight: 500; transition: all 0.2s; font-family: 'DM Sans', sans-serif; }
.cat-btn.active { background: ${COLORS.night}; color: white; border-color: ${COLORS.night}; }
.checklist-items { background: white; border-radius: 16px; padding: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); margin-bottom: 20px; }
.check-item { display: flex; align-items: center; gap: 14px; padding: 14px 16px; border-radius: 10px; transition: background 0.15s; }
.check-item:hover { background: ${COLORS.sand}; }
.check-item input[type=checkbox] { width: 18px; height: 18px; cursor: pointer; accent-color: ${COLORS.clay}; }
.check-item .item-name { flex: 1; font-size: 14px; }
.check-item .item-name.done { text-decoration: line-through; color: ${COLORS.muted}; }
.check-item .item-cat { font-size: 11px; color: ${COLORS.muted}; background: ${COLORS.sand}; padding: 3px 8px; border-radius: 10px; }

/* PROFILE */
.profile-wrap { padding: 36px 40px; max-width: 640px; }
.profile-header { background: white; border-radius: 20px; padding: 32px; display: flex; align-items: center; gap: 24px; margin-bottom: 24px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
.avatar { width: 80px; height: 80px; border-radius: 50%; background: linear-gradient(135deg, ${COLORS.clay}, ${COLORS.gold}); display: flex; align-items: center; justify-content: center; font-size: 32px; color: white; font-family: 'Playfair Display', serif; font-weight: 700; flex-shrink: 0; }
.profile-header h2 { font-size: 24px; margin-bottom: 4px; }
.profile-header p { color: ${COLORS.muted}; font-size: 14px; }
.settings-section { background: white; border-radius: 16px; padding: 24px; margin-bottom: 16px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
.settings-section h3 { font-size: 16px; margin-bottom: 18px; padding-bottom: 12px; border-bottom: 1px solid ${COLORS.sand}; }

/* NOTES */
.notes-wrap { padding: 36px 40px; display: grid; grid-template-columns: 260px 1fr; gap: 24px; min-height: calc(100vh - 60px); }
.notes-list { }
.note-item { background: white; border-radius: 12px; padding: 16px; margin-bottom: 10px; cursor: pointer; border: 2px solid transparent; transition: all 0.2s; box-shadow: 0 1px 4px rgba(0,0,0,0.05); }
.note-item.active, .note-item:hover { border-color: ${COLORS.clay}; }
.note-item h4 { font-size: 14px; font-family: 'DM Sans', sans-serif; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.note-item p { font-size: 12px; color: ${COLORS.muted}; margin-top: 4px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.note-editor { background: white; border-radius: 16px; padding: 28px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }
.note-editor textarea { width: 100%; border: none; outline: none; font-family: 'DM Sans', sans-serif; font-size: 15px; line-height: 1.8; min-height: 400px; resize: none; color: ${COLORS.night}; }

/* SHARED VIEW */
.shared-wrap { max-width: 700px; margin: 0 auto; padding: 40px 20px; }
.shared-hero { background: linear-gradient(160deg, ${COLORS.forest}, ${COLORS.night}); color: white; border-radius: 20px; padding: 40px; text-align: center; margin-bottom: 32px; }
.shared-hero h1 { font-size: 36px; margin-bottom: 8px; }
.shared-hero p { opacity: 0.7; }
.shared-actions { display: flex; justify-content: center; gap: 12px; margin-top: 24px; }

/* SEARCH */
.search-wrap { padding: 36px 40px; }
.search-box { display: flex; gap: 12px; margin-bottom: 28px; }
.search-box input { flex: 1; padding: 14px 20px; border: 2px solid #E8E4DC; border-radius: 12px; font-size: 15px; font-family: 'DM Sans', sans-serif; outline: none; }
.search-box input:focus { border-color: ${COLORS.clay}; }
.filters { display: flex; gap: 10px; margin-bottom: 24px; flex-wrap: wrap; }
.filter-btn { padding: 8px 18px; border-radius: 20px; border: 1.5px solid #E8E4DC; background: white; cursor: pointer; font-size: 13px; font-weight: 500; font-family: 'DM Sans', sans-serif; transition: all 0.2s; }
.filter-btn.active { background: ${COLORS.clay}; color: white; border-color: ${COLORS.clay}; }
.results-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 16px; }
.result-card { background: white; border-radius: 14px; padding: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1.5px solid transparent; transition: all 0.2s; }
.result-card:hover { border-color: ${COLORS.sky}; transform: translateY(-2px); }
.result-card .rc-top { display: flex; align-items: center; gap: 12px; margin-bottom: 12px; }
.result-card .rc-emoji { font-size: 32px; }
.result-card h4 { font-size: 15px; }
.result-card p { font-size: 12px; color: ${COLORS.muted}; }
.result-card .rc-tags { display: flex; gap: 6px; flex-wrap: wrap; margin: 10px 0; }
.result-card .tag { background: ${COLORS.sand}; padding: 4px 10px; border-radius: 8px; font-size: 11px; color: ${COLORS.muted}; }
.result-card .rc-footer { display: flex; align-items: center; justify-content: space-between; margin-top: 10px; }
.result-card .price { font-weight: 700; color: ${COLORS.forest}; font-size: 14px; }

/* EMPTY */
.empty-state { text-align: center; padding: 60px 20px; color: ${COLORS.muted}; }
.empty-state .emoji { font-size: 64px; margin-bottom: 16px; }
.empty-state h3 { font-size: 20px; color: ${COLORS.night}; margin-bottom: 8px; }
.empty-state p { font-size: 14px; max-width: 300px; margin: 0 auto 24px; }

/* Toast */
.toast { position: fixed; bottom: 24px; right: 24px; background: ${COLORS.night}; color: white; padding: 14px 20px; border-radius: 12px; font-size: 14px; z-index: 999; animation: slideUp 0.3s ease; box-shadow: 0 8px 24px rgba(0,0,0,0.2); }
@keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
`;

// ── DATA ──────────────────────────────────────────────────────────────────────
const SAMPLE_TRIPS = [
  { id: 1, name: "Euro Summer 2026", emoji: "🏰", start: "2026-07-01", end: "2026-07-21", stops: ["Paris", "Rome", "Barcelona"], budget: 4500, spent: 3200, status: "upcoming", desc: "Exploring the best of Western Europe" },
  { id: 2, name: "Bali Retreat", emoji: "🌴", start: "2026-09-10", end: "2026-09-24", stops: ["Ubud", "Seminyak", "Nusa Dua"], budget: 2800, spent: 1400, status: "planning", desc: "Wellness and culture in Indonesia" },
  { id: 3, name: "Japan Cherry Blossom", emoji: "🌸", start: "2026-03-20", end: "2026-04-05", stops: ["Tokyo", "Kyoto", "Osaka"], budget: 5500, spent: 5200, status: "completed", desc: "Sakura season adventure" },
];

const SAMPLE_ACTIVITIES = {
  "Paris": [
    { id: 1, name: "Eiffel Tower Visit", time: "9:00 AM", cost: 26, icon: "🗼", type: "Sightseeing", duration: "2 hrs" },
    { id: 2, name: "Louvre Museum", time: "2:00 PM", cost: 17, icon: "🎨", type: "Culture", duration: "3 hrs" },
    { id: 3, name: "Seine River Cruise", time: "7:00 PM", cost: 22, icon: "⛵", type: "Experience", duration: "1 hr" },
  ],
  "Rome": [
    { id: 4, name: "Colosseum Tour", time: "10:00 AM", cost: 18, icon: "🏟", type: "History", duration: "2 hrs" },
    { id: 5, name: "Vatican Museums", time: "2:00 PM", cost: 27, icon: "⛪", type: "Culture", duration: "3 hrs" },
  ],
  "Barcelona": [
    { id: 6, name: "Sagrada Familia", time: "9:00 AM", cost: 26, icon: "⛪", type: "Architecture", duration: "2 hrs" },
    { id: 7, name: "Park Güell", time: "3:00 PM", cost: 10, icon: "🌳", type: "Outdoors", duration: "1.5 hrs" },
  ],
};

const CITIES = [
  { name: "Tokyo", country: "Japan", emoji: "🗼", cost: "$$$$", pop: 98 },
  { name: "Paris", country: "France", emoji: "🏰", cost: "$$$", pop: 96 },
  { name: "Bali", country: "Indonesia", emoji: "🌴", cost: "$$", pop: 94 },
  { name: "New York", country: "USA", emoji: "🗽", cost: "$$$$", pop: 93 },
  { name: "Rome", country: "Italy", emoji: "🏛", cost: "$$$", pop: 91 },
  { name: "Bangkok", country: "Thailand", emoji: "🛕", cost: "$$", pop: 90 },
  { name: "Barcelona", country: "Spain", emoji: "🎨", cost: "$$$", pop: 89 },
  { name: "Sydney", country: "Australia", emoji: "🦘", cost: "$$$$", pop: 87 },
  { name: "Dubai", country: "UAE", emoji: "🌇", cost: "$$$$", pop: 85 },
  { name: "Lisbon", country: "Portugal", emoji: "🏖", cost: "$$", pop: 84 },
  { name: "Cape Town", country: "S.Africa", emoji: "🦁", cost: "$$", pop: 82 },
  { name: "Kyoto", country: "Japan", emoji: "🌸", cost: "$$$", pop: 88 },
];

const ACTIVITIES_DB = [
  { id: 1, name: "City Walking Tour", type: "Sightseeing", city: "Paris", cost: 15, duration: "3 hrs", emoji: "👣", desc: "Explore historic neighborhoods with a local guide" },
  { id: 2, name: "Cooking Class", type: "Food", city: "Rome", cost: 65, duration: "4 hrs", emoji: "👨‍🍳", desc: "Learn authentic Italian recipes from a local chef" },
  { id: 3, name: "Sunset Boat Trip", type: "Adventure", city: "Bali", cost: 45, duration: "2 hrs", emoji: "⛵", desc: "Sail along the coast as the sun sets" 