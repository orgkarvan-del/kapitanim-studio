<!DOCTYPE html>
<html lang="ug" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="theme-color" content="#0f172a">
    <title>رېستوران باشقۇرۇش سېستىمىسى</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Arabic Typesetting', 'Traditional Arabic', Tahoma, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        html, body {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: white;
            min-height: 100vh;
            overflow-x: hidden;
        }
        
        /* ئاساسىي رەڭلەر */
        :root {
            --primary: #6366f1;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --info: #3b82f6;
        }
        
        /* Glass ئۈنۈمى */
        .glass {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        /* كۇنۇپكىلار */
        .btn {
            padding: 12px 20px;
            border-radius: 12px;
            border: none;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        .btn:active { transform: scale(0.95); }
        .btn-primary {
            background: linear-gradient(135deg, #6366f1, #8b5cf6);
            color: white;
            box-shadow: 0 4px 15px rgba(99, 102, 241, 0.4);
        }
        .btn-success {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
        }
        .btn-danger {
            background: rgba(239, 68, 68, 0.2);
            color: #ef4444;
        }
        .btn-warning {
            background: rgba(245, 158, 11, 0.2);
            color: #f59e0b;
        }
        
        /* كىرگۈزۈش */
        input, select, textarea {
            width: 100%;
            padding: 12px 16px;
            border-radius: 12px;
            border: 1px solid rgba(255,255,255,0.1);
            background: rgba(255,255,255,0.05);
            color: white;
            font-size: 16px;
            outline: none;
            transition: all 0.3s;
        }
        input:focus, select:focus, textarea:focus {
            border-color: #6366f1;
            box-shadow: 0 0 20px rgba(99, 102, 241, 0.3);
        }
        select option {
            background: #1e293b;
            color: white;
        }
        
        /* كارتا */
        .card {
            background: rgba(30, 41, 59, 0.8);
            border-radius: 16px;
            padding: 16px;
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.3s;
        }
        .card:active { transform: scale(0.98); }
        
        /* Header */
        .header {
            position: sticky;
            top: 0;
            z-index: 100;
            padding: 12px 16px;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        /* ئاستىقى يول باشلاش */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(10px);
            border-top: 1px solid rgba(255,255,255,0.1);
            display: flex;
            justify-content: space-around;
            padding: 8px 0;
            padding-bottom: env(safe-area-inset-bottom, 8px);
            z-index: 100;
        }
        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            padding: 8px 16px;
            color: #94a3b8;
            text-decoration: none;
            font-size: 10px;
            transition: all 0.3s;
            cursor: pointer;
            border: none;
            background: none;
        }
        .nav-item.active { color: #6366f1; }
        .nav-item span:first-child { font-size: 20px; }
        
        /* بەت */
        .page {
            display: none;
            padding: 16px;
            padding-bottom: 100px;
            min-height: calc(100vh - 60px);
        }
        .page.active { display: block; }
        
        /* Modal */
        .modal {
            display: none;
            position: fixed;
            inset: 0;
            z-index: 200;
            align-items: flex-end;
            justify-content: center;
        }
        .modal.active { display: flex; }
        .modal-overlay {
            position: absolute;
            inset: 0;
            background: rgba(0,0,0,0.7);
        }
        .modal-content {
            position: relative;
            background: #1e293b;
            border-radius: 24px 24px 0 0;
            padding: 24px;
            width: 100%;
            max-height: 85vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease;
        }
        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }
        .modal-handle {
            width: 40px;
            height: 4px;
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
            margin: 0 auto 16px;
        }
        
        /* Grid */
        .grid { display: grid; gap: 12px; }
        .grid-2 { grid-template-columns: repeat(2, 1fr); }
        .grid-3 { grid-template-columns: repeat(3, 1fr); }
        .grid-4 { grid-template-columns: repeat(4, 1fr); }
        
        /* Flex */
        .flex { display: flex; }
        .flex-col { flex-direction: column; }
        .items-center { align-items: center; }
        .justify-between { justify-content: space-between; }
        .justify-center { justify-content: center; }
        .gap-1 { gap: 4px; }
        .gap-2 { gap: 8px; }
        .gap-3 { gap: 12px; }
        .gap-4 { gap: 16px; }
        .flex-1 { flex: 1; }
        
        /* Spacing */
        .p-2 { padding: 8px; }
        .p-3 { padding: 12px; }
        .p-4 { padding: 16px; }
        .mb-2 { margin-bottom: 8px; }
        .mb-3 { margin-bottom: 12px; }
        .mb-4 { margin-bottom: 16px; }
        .mt-2 { margin-top: 8px; }
        .mt-3 { margin-top: 12px; }
        
        /* Text */
        .text-xs { font-size: 10px; }
        .text-sm { font-size: 12px; }
        .text-lg { font-size: 18px; }
        .text-xl { font-size: 20px; }
        .text-2xl { font-size: 24px; }
        .text-3xl { font-size: 30px; }
        .text-center { text-align: center; }
        .font-bold { font-weight: bold; }
        
        /* Colors */
        .text-green { color: #10b981; }
        .text-red { color: #ef4444; }
        .text-yellow { color: #f59e0b; }
        .text-blue { color: #3b82f6; }
        .text-purple { color: #8b5cf6; }
        .text-orange { color: #f97316; }
        .text-gray { color: #94a3b8; }
        .text-white { color: white; }
        
        .bg-green { background: rgba(16, 185, 129, 0.2); }
        .bg-red { background: rgba(239, 68, 68, 0.2); }
        .bg-yellow { background: rgba(245, 158, 11, 0.2); }
        .bg-blue { background: rgba(59, 130, 246, 0.2); }
        .bg-purple { background: rgba(139, 92, 246, 0.2); }
        
        /* Border */
        .rounded { border-radius: 8px; }
        .rounded-lg { border-radius: 12px; }
        .rounded-xl { border-radius: 16px; }
        .rounded-2xl { border-radius: 24px; }
        .rounded-full { border-radius: 50%; }
        
        /* Status */
        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        
        /* ھاۋادا لەيلەش ئۈنۈمى */
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-8px); }
        }
        @keyframes floatSlow {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-5px) rotate(2deg); }
        }
        @keyframes glow {
            0%, 100% { box-shadow: 0 0 20px rgba(99, 102, 241, 0.3); }
            50% { box-shadow: 0 0 40px rgba(99, 102, 241, 0.6); }
        }
        @keyframes shimmer {
            0% { background-position: -200% center; }
            100% { background-position: 200% center; }
        }
        
        /* لەيلىگەن ئايكون */
        .floating-icon {
            animation: float 3s ease-in-out infinite;
            filter: drop-shadow(0 10px 20px rgba(0,0,0,0.3));
        }
        .floating-icon-slow {
            animation: floatSlow 4s ease-in-out infinite;
            filter: drop-shadow(0 8px 16px rgba(0,0,0,0.25));
        }
        
        /* پارقىراق ئۈنۈم */
        .glow-effect {
            animation: glow 2s ease-in-out infinite;
        }
        
        /* Glass ئىلغار ئۈنۈمى */
        .glass-card {
            background: rgba(30, 41, 59, 0.6);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 
                0 8px 32px rgba(0,0,0,0.3),
                inset 0 1px 0 rgba(255,255,255,0.1);
        }
        
        /* نېئون پارقىراق */
        .neon-text {
            text-shadow: 
                0 0 10px currentColor,
                0 0 20px currentColor,
                0 0 40px currentColor;
        }
        
        /* 3D كۇنۇپكا */
        .btn-3d {
            transform: translateY(0);
            box-shadow: 
                0 4px 0 rgba(0,0,0,0.3),
                0 8px 20px rgba(0,0,0,0.2);
            transition: all 0.15s ease;
        }
        .btn-3d:active {
            transform: translateY(4px);
            box-shadow: 
                0 0 0 rgba(0,0,0,0.3),
                0 2px 10px rgba(0,0,0,0.2);
        }
        
        /* ھەشەمەتلىك كارتا */
        .luxury-card {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.8) 0%, rgba(15, 23, 42, 0.9) 100%);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 20px;
            box-shadow: 
                0 20px 60px rgba(0,0,0,0.4),
                0 0 0 1px rgba(255,255,255,0.05),
                inset 0 1px 0 rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
        }
        .luxury-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shimmer 3s infinite;
        }
        
        /* ئايكون قاچىسى */
        .icon-container {
            width: 56px;
            height: 56px;
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 28px;
            position: relative;
            animation: floatSlow 3s ease-in-out infinite;
        }
        .icon-container::after {
            content: '';
            position: absolute;
            bottom: -8px;
            left: 50%;
            transform: translateX(-50%);
            width: 70%;
            height: 8px;
            background: radial-gradient(ellipse, rgba(0,0,0,0.3), transparent);
            filter: blur(4px);
        }
        
        /* چوڭ ستاتىستىكا سانى */
        .stat-number {
            font-size: 28px;
            font-weight: bold;
            background: linear-gradient(135deg, currentColor, rgba(255,255,255,0.8));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        /* تائام كارتىسى ھەشەمەتلىك */
        .food-card-luxury {
            background: linear-gradient(145deg, rgba(30, 41, 59, 0.9), rgba(15, 23, 42, 0.95));
            border-radius: 16px;
            padding: 16px 12px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
            box-shadow: 
                0 10px 30px rgba(0,0,0,0.3),
                inset 0 1px 0 rgba(255,255,255,0.1);
        }
        .food-card-luxury::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 50%;
            background: linear-gradient(180deg, rgba(255,255,255,0.1), transparent);
            border-radius: 16px 16px 0 0;
        }
        .food-card-luxury:hover {
            transform: translateY(-8px) scale(1.02);
            box-shadow: 
                0 20px 40px rgba(0,0,0,0.4),
                0 0 30px rgba(99, 102, 241, 0.3);
            border-color: rgba(99, 102, 241, 0.5);
        }
        .food-card-luxury:active {
            transform: translateY(-4px) scale(0.98);
        }
        .food-card-luxury .emoji {
            font-size: 36px;
            display: block;
            margin-bottom: 8px;
            animation: floatSlow 2.5s ease-in-out infinite;
            filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
        }
        
        /* ناۋبار ھەشەمەتلىك */
        .nav-item-luxury {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            padding: 10px 16px;
            color: #94a3b8;
            text-decoration: none;
            font-size: 11px;
            transition: all 0.3s ease;
            cursor: pointer;
            border: none;
            background: none;
            position: relative;
        }
        .nav-item-luxury span:first-child {
            font-size: 24px;
            transition: all 0.3s ease;
        }
        .nav-item-luxury.active {
            color: #6366f1;
        }
        .nav-item-luxury.active span:first-child {
            animation: float 2s ease-in-out infinite;
            filter: drop-shadow(0 0 10px rgba(99, 102, 241, 0.5));
        }
        .nav-item-luxury::before {
            content: '';
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%) scaleX(0);
            width: 40px;
            height: 3px;
            background: linear-gradient(90deg, #6366f1, #8b5cf6);
            border-radius: 0 0 4px 4px;
            transition: transform 0.3s ease;
        }
        .nav-item-luxury.active::before {
            transform: translateX(-50%) scaleX(1);
        }
        
        /* ئورۇن كارتىسى ھەشەمەتلىك */
        .location-card-luxury {
            border-radius: 16px;
            padding: 16px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        .location-card-luxury.free {
            background: linear-gradient(135deg, rgba(16, 185, 129, 0.2), rgba(5, 150, 105, 0.1));
            border: 2px solid rgba(16, 185, 129, 0.5);
            box-shadow: 0 0 20px rgba(16, 185, 129, 0.2);
        }
        .location-card-luxury.occupied {
            background: linear-gradient(135deg, rgba(239, 68, 68, 0.2), rgba(185, 28, 28, 0.1));
            border: 2px solid rgba(239, 68, 68, 0.5);
            box-shadow: 0 0 20px rgba(239, 68, 68, 0.2);
        }
        .location-card-luxury:hover {
            transform: translateY(-5px) scale(1.02);
        }
        .location-card-luxury .icon {
            font-size: 32px;
            display: block;
            margin-bottom: 8px;
            animation: floatSlow 3s ease-in-out infinite;
        }
        
        /* ساندۇق كارتىسى */
        .cashbox-card {
            background: linear-gradient(145deg, rgba(30, 41, 59, 0.9), rgba(15, 23, 42, 0.95));
            border-radius: 20px;
            padding: 20px;
            position: relative;
            overflow: hidden;
            box-shadow: 0 15px 40px rgba(0,0,0,0.3);
        }
        .cashbox-card::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -50%;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, rgba(255,255,255,0.1), transparent);
        }
        .cashbox-card .icon {
            font-size: 40px;
            animation: float 3s ease-in-out infinite;
            filter: drop-shadow(0 8px 16px rgba(0,0,0,0.3));
        }
        
        /* يۈكلىنىش ئېكرانى */
        .loading-luxury {
            background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
            background-size: 400% 400%;
            animation: gradientShift 3s ease infinite;
        }
        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }
        
        /* كىرگۈزۈش رامكىسى ھەشەمەتلىك */
        .input-luxury {
            width: 100%;
            padding: 14px 18px;
            border-radius: 14px;
            border: 2px solid rgba(255,255,255,0.1);
            background: rgba(255,255,255,0.05);
            color: white;
            font-size: 16px;
            outline: none;
            transition: all 0.3s ease;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
        }
        .input-luxury:focus {
            border-color: #6366f1;
            box-shadow: 
                0 0 0 4px rgba(99, 102, 241, 0.2),
                inset 0 2px 4px rgba(0,0,0,0.2);
        }
        
        /* Scrollbar */
        ::-webkit-scrollbar { width: 4px; height: 4px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #6366f1; border-radius: 4px; }
        
        /* Login Screen */
        .login-screen {
            position: fixed;
            inset: 0;
            background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #0f172a 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
        }
        .login-screen.hidden { display: none; }
        .login-container {
            text-align: center;
            padding: 24px;
            max-width: 360px;
            width: 100%;
        }
        .login-logo {
            font-size: 80px;
            margin-bottom: 16px;
            animation: float 3s ease-in-out infinite;
            filter: drop-shadow(0 10px 30px rgba(99, 102, 241, 0.5));
        }
        .login-title {
            font-size: 28px;
            font-weight: bold;
            margin-bottom: 8px;
            background: linear-gradient(135deg, #fff, #6366f1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .login-subtitle {
            color: #94a3b8;
            font-size: 14px;
            margin-bottom: 32px;
        }
        .login-form {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(20px);
            border-radius: 24px;
            padding: 24px;
            border: 1px solid rgba(255,255,255,0.1);
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
        }
        .pin-display {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin-bottom: 16px;
        }
        .pin-dot {
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: rgba(255,255,255,0.1);
            border: 2px solid rgba(255,255,255,0.2);
            transition: all 0.3s;
        }
        .pin-dot.filled {
            background: #6366f1;
            border-color: #6366f1;
            box-shadow: 0 0 15px rgba(99, 102, 241, 0.5);
        }
        .pin-dot.error {
            background: #ef4444;
            border-color: #ef4444;
            animation: shake 0.5s;
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
        .login-hint {
            color: #94a3b8;
            font-size: 12px;
            margin-bottom: 20px;
        }
        .pin-keypad {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            max-width: 280px;
            margin: 0 auto;
        }
        .pin-key {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            border: 2px solid rgba(255,255,255,0.1);
            background: rgba(255,255,255,0.05);
            color: white;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto;
        }
        .pin-key:hover {
            background: rgba(99, 102, 241, 0.3);
            border-color: #6366f1;
            transform: scale(1.05);
        }
        .pin-key:active {
            transform: scale(0.95);
            background: #6366f1;
        }
        .pin-key-clear {
            background: rgba(239, 68, 68, 0.2);
            border-color: rgba(239, 68, 68, 0.3);
            color: #ef4444;
            font-size: 24px;
        }
        .pin-key-clear:hover {
            background: rgba(239, 68, 68, 0.4);
            border-color: #ef4444;
        }
        .pin-key-enter {
            background: rgba(16, 185, 129, 0.2);
            border-color: rgba(16, 185, 129, 0.3);
            color: #10b981;
            font-size: 28px;
        }
        .pin-key-enter:hover {
            background: rgba(16, 185, 129, 0.4);
            border-color: #10b981;
        }
        .login-error {
            color: #ef4444;
            font-size: 12px;
            margin-top: 16px;
            min-height: 18px;
        }
        .login-forgot {
            background: none;
            border: none;
            color: #6366f1;
            font-size: 12px;
            cursor: pointer;
            margin-top: 16px;
            text-decoration: underline;
        }
        .login-footer {
            margin-top: 24px;
            color: #64748b;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        /* Session Timer */
        .session-timer {
            position: fixed;
            top: 70px;
            left: 16px;
            background: rgba(245, 158, 11, 0.2);
            border: 1px solid rgba(245, 158, 11, 0.5);
            border-radius: 8px;
            padding: 4px 12px;
            font-size: 10px;
            color: #f59e0b;
            z-index: 100;
            display: none;
        }
        .session-timer.warning {
            background: rgba(239, 68, 68, 0.2);
            border-color: rgba(239, 68, 68, 0.5);
            color: #ef4444;
            animation: pulse 1s infinite;
        }
        
        /* سۈزگۈچ */
        .filter-scroll {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding-bottom: 8px;
            -webkit-overflow-scrolling: touch;
        }
        .filter-scroll::-webkit-scrollbar { display: none; }
        .filter-btn {
            white-space: nowrap;
            padding: 8px 16px;
            border-radius: 20px;
            border: none;
            background: rgba(255,255,255,0.05);
            color: #94a3b8;
            font-size: 12px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .filter-btn.active {
            background: #6366f1;
            color: white;
        }
        
        /* تائام كارتىسى */
        .food-card {
            border-radius: 12px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid transparent;
        }
        .food-card:active {
            transform: scale(0.95);
            border-color: rgba(255,255,255,0.3);
        }
        .food-card .emoji {
            font-size: 28px;
            margin-bottom: 4px;
        }
        .food-card .name {
            font-size: 11px;
            font-weight: bold;
            margin-bottom: 4px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .food-card .price {
            font-size: 12px;
            font-weight: bold;
            background: rgba(0,0,0,0.3);
            padding: 2px 8px;
            border-radius: 10px;
            display: inline-block;
        }
        
        /* ئورۇن كارتىسى */
        .location-card {
            border-radius: 12px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        .location-card:active { transform: scale(0.95); }
        .location-card.free {
            background: rgba(16, 185, 129, 0.2);
            border: 2px solid rgba(16, 185, 129, 0.5);
        }
        .location-card.occupied {
            background: rgba(239, 68, 68, 0.2);
            border: 2px solid rgba(239, 68, 68, 0.5);
        }
        
        /* سېۋەت */
        .cart-fixed {
            position: fixed;
            bottom: 70px;
            left: 0;
            right: 0;
            background: rgba(15, 23, 42, 0.98);
            border-top: 1px solid rgba(255,255,255,0.1);
            padding: 12px;
            z-index: 90;
            animation: slideUp 0.3s ease;
        }
        .cart-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 8px;
            background: rgba(255,255,255,0.05);
            border-radius: 8px;
            margin-bottom: 8px;
        }
        .qty-btn {
            width: 28px;
            height: 28px;
            border-radius: 8px;
            border: none;
            font-weight: bold;
            cursor: pointer;
        }
        .qty-btn.minus { background: rgba(239, 68, 68, 0.2); color: #ef4444; }
        .qty-btn.plus { background: rgba(16, 185, 129, 0.2); color: #10b981; }
        
        /* تالون */
        .receipt {
            background: white;
            color: black;
            padding: 16px;
            border-radius: 8px;
            font-family: monospace;
        }
        .receipt h2 { margin-bottom: 8px; }
        .receipt hr { border: none; border-top: 1px dashed #ccc; margin: 8px 0; }
        .receipt table { width: 100%; font-size: 12px; }
        .receipt th, .receipt td { padding: 4px; text-align: right; }
        
        /* Loading */
        .loading-screen {
            position: fixed;
            inset: 0;
            background: linear-gradient(135deg, #6366f1, #8b5cf6);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .loading-screen.hidden { display: none; }
        
        /* Toast */
        .toast {
            position: fixed;
            top: 80px;
            left: 16px;
            right: 16px;
            padding: 12px 16px;
            border-radius: 12px;
            background: rgba(30, 41, 59, 0.95);
            border: 1px solid rgba(255,255,255,0.1);
            transform: translateY(-100px);
            opacity: 0;
            transition: all 0.3s;
            z-index: 300;
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .toast.show {
            transform: translateY(0);
            opacity: 1;
        }
        
        /* Sidebar */
        .sidebar-overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.5);
            z-index: 150;
        }
        .sidebar-overlay.active { display: block; }
        .sidebar {
            position: fixed;
            top: 0;
            right: 0;
            width: 280px;
            height: 100%;
            background: #1e293b;
            z-index: 151;
            transform: translateX(100%);
            transition: transform 0.3s;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }
        .sidebar.active { transform: translateX(0); }
        .sidebar-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 12px 16px;
            color: #94a3b8;
            cursor: pointer;
            transition: all 0.3s;
            border: none;
            background: none;
            width: 100%;
            text-align: right;
        }
        .sidebar-item:hover, .sidebar-item.active {
            background: rgba(99, 102, 241, 0.2);
            color: white;
            border-right: 3px solid #6366f1;
        }
        
        /* Responsive */
        @media (min-width: 768px) {
            .grid-md-4 { grid-template-columns: repeat(4, 1fr); }
            .grid-md-6 { grid-template-columns: repeat(6, 1fr); }
        }
        @media (min-width: 1024px) {
            .grid-lg-4 { grid-template-columns: repeat(4, 1fr); }
            .container { max-width: 1200px; margin: 0 auto; }
        }
        @media (max-width: 380px) {
            .food-card .emoji { font-size: 24px; }
            .food-card .name { font-size: 10px; }
        }
    </style>
</head>
<body>
    <!-- Login Screen -->
    <div class="login-screen" id="loginScreen">
        <div class="login-container">
            <div class="login-logo">🍽️</div>
            <h1 class="login-title">رېستوران باشقۇرۇش</h1>
            <p class="login-subtitle">سېستىمىغا كىرىش</p>
            
            <div class="login-form">
                <div class="pin-display" id="pinDisplay">
                    <span class="pin-dot"></span>
                    <span class="pin-dot"></span>
                    <span class="pin-dot"></span>
                    <span class="pin-dot"></span>
                    <span class="pin-dot"></span>
                    <span class="pin-dot"></span>
                </div>
                
                <p class="login-hint" id="loginHint">6 خانىلىق شىفىرنى كىرگۈزۈڭ</p>
                
                <div class="pin-keypad">
                    <button class="pin-key" onclick="enterPin('1')">1</button>
                    <button class="pin-key" onclick="enterPin('2')">2</button>
                    <button class="pin-key" onclick="enterPin('3')">3</button>
                    <button class="pin-key" onclick="enterPin('4')">4</button>
                    <button class="pin-key" onclick="enterPin('5')">5</button>
                    <button class="pin-key" onclick="enterPin('6')">6</button>
                    <button class="pin-key" onclick="enterPin('7')">7</button>
                    <button class="pin-key" onclick="enterPin('8')">8</button>
                    <button class="pin-key" onclick="enterPin('9')">9</button>
                    <button class="pin-key pin-key-clear" onclick="clearPin()">⌫</button>
                    <button class="pin-key" onclick="enterPin('0')">0</button>
                    <button class="pin-key pin-key-enter" onclick="submitPin()">✓</button>
                </div>
                
                <p class="login-error" id="loginError"></p>
                
                <button class="login-forgot" onclick="showForgotPin()">شىفىرنى ئۇنتۇتتىڭىزمۇ؟</button>
            </div>
            
            <div class="login-footer">
                <span>🔒</span> بىخەتەر سېستىما
            </div>
        </div>
    </div>

    <!-- Loading -->
    <div class="loading-screen loading-luxury" id="loadingScreen" style="display:none;">
        <div class="text-center">
            <div style="font-size: 80px; margin-bottom: 20px;" class="floating-icon">🍽️</div>
            <h1 style="font-size:24px;font-weight:bold;margin-bottom:12px;background:linear-gradient(135deg,#fff,#6366f1);-webkit-background-clip:text;-webkit-text-fill-color:transparent;">رېستوران باشقۇرۇش</h1>
            <p class="text-gray text-sm">سېستىما يۈكلىنىۋاتىدۇ...</p>
            <div style="width:150px;height:4px;background:rgba(255,255,255,0.1);border-radius:4px;margin:20px auto;overflow:hidden;">
                <div style="width:30%;height:100%;background:linear-gradient(90deg,#6366f1,#8b5cf6);border-radius:4px;animation:shimmer 1.5s infinite;"></div>
            </div>
        </div>
    </div>

    <!-- Session Timer -->
    <div class="session-timer" id="sessionTimer">⏱️ <span id="sessionTime">30:00</span></div>
    
    <!-- Lock Button -->
    <button onclick="lockSystem()" id="lockBtn" style="position:fixed;top:70px;right:16px;background:rgba(239,68,68,0.2);border:1px solid rgba(239,68,68,0.5);color:#ef4444;padding:6px 12px;border-radius:8px;font-size:11px;cursor:pointer;z-index:100;display:none;">🔒 قۇلۇپلاش</button>

    <!-- Toast -->
    <div class="toast" id="toast">
        <span id="toastIcon">✅</span>
        <span id="toastMessage"></span>
    </div>

    <!-- Header -->
    <header class="header">
        <div class="flex items-center justify-between">
            <button onclick="toggleSidebar()" style="background:none;border:none;color:white;font-size:24px;cursor:pointer;">☰</button>
            <div class="flex items-center gap-3">
                <span id="currentTime" class="font-bold text-lg"></span>
                <div id="connectionStatus" class="flex items-center gap-1 text-xs bg-green rounded-full" style="padding:4px 12px;">
                    <span class="status-dot" style="background:#10b981;"></span>
                    <span class="text-green">ئۇلانغان</span>
                </div>
            </div>
        </div>
        <div class="text-center mt-1 text-sm text-gray">
            <span id="currentDate"></span>
        </div>
    </header>

    <!-- Sidebar Overlay -->
    <div class="sidebar-overlay" id="sidebarOverlay" onclick="toggleSidebar()"></div>
    
    <!-- Sidebar -->
    <aside class="sidebar" id="sidebar">
        <div class="p-3" style="border-bottom:1px solid rgba(255,255,255,0.1);display:flex;align-items:center;justify-content:space-between;">
            <span class="font-bold text-lg">📋 تىزىملىك</span>
            <button onclick="toggleSidebar()" style="background:none;border:none;color:white;font-size:20px;cursor:pointer;">✕</button>
        </div>
        <nav class="p-2" style="flex:1;overflow-y:auto;">
            <button class="sidebar-item active" onclick="showPage('dashboard')">📊 باش بەت</button>
            <button class="sidebar-item" onclick="showPage('pos')">💰 سېتىش</button>
            <button class="sidebar-item" onclick="showPage('menu')">📖 تائام تىزىملىكى</button>
            <button class="sidebar-item" onclick="showPage('orders')">📋 زاكازلار</button>
            <button class="sidebar-item" onclick="showPage('tables')">🪑 ئۈستەللەر</button>
            <button class="sidebar-item" onclick="showPage('rooms')">🚪 ئايرىمخانىلار</button>
            <button class="sidebar-item" onclick="showPage('employees')">👨‍🍳 خىزمەتچىلەر</button>
            <button class="sidebar-item" onclick="showPage('expenses')">💸 چىقىملار</button>
            <button class="sidebar-item" onclick="showPage('cashbox')">🏦 ساندۇقلار</button>
            <button class="sidebar-item" onclick="showPage('reports')">📈 دوكلاتلار</button>
            <button class="sidebar-item" onclick="showPage('settings')">⚙️ تەڭشەكلەر</button>
            <div style="border-top:1px solid rgba(255,255,255,0.1);margin:8px 0;"></div>
            <button class="sidebar-item" onclick="openCustomerMode()" style="background:linear-gradient(135deg,rgba(16,185,129,0.2),rgba(5,150,105,0.1));border:1px solid rgba(16,185,129,0.3);">
                📱 خېرىدار زاكاز بېتى
            </button>
        </nav>
        <div class="p-3" style="border-top:1px solid rgba(255,255,255,0.1);margin-top:auto;">
            <div class="flex items-center justify-between mb-2">
                <span class="text-xs text-gray">💾 ساقلاش:</span>
                <span class="text-xs font-bold text-green" id="storageInfo">0 MB</span>
            </div>
            <div style="height:6px;background:rgba(255,255,255,0.1);border-radius:4px;overflow:hidden;">
                <div id="storageBar" style="height:100%;background:linear-gradient(90deg,#10b981,#059669);width:0%;transition:width 0.3s;border-radius:4px;"></div>
            </div>
        </div>
    </aside>

    <!-- Pages -->
    <main>
        <!-- Dashboard -->
        <div class="page active" id="page-dashboard">
            <div class="grid grid-2 mb-4">
                <div class="luxury-card" style="padding:16px;">
                    <div class="icon-container" style="background:linear-gradient(135deg,rgba(16,185,129,0.3),rgba(5,150,105,0.2));margin-bottom:12px;">📈</div>
                    <h3 class="stat-number text-green" id="todayRevenue">$0</h3>
                    <p class="text-xs text-gray" style="margin-top:4px;">بۈگۈنكى كىرىم</p>
                </div>
                <div class="luxury-card" style="padding:16px;">
                    <div class="icon-container" style="background:linear-gradient(135deg,rgba(239,68,68,0.3),rgba(185,28,28,0.2));margin-bottom:12px;">📉</div>
                    <h3 class="stat-number text-red" id="todayExpense">$0</h3>
                    <p class="text-xs text-gray" style="margin-top:4px;">بۈگۈنكى چىقىم</p>
                </div>
                <div class="luxury-card" style="padding:16px;">
                    <div class="icon-container" style="background:linear-gradient(135deg,rgba(59,130,246,0.3),rgba(29,78,216,0.2));margin-bottom:12px;">📋</div>
                    <h3 class="stat-number text-blue" id="todayOrders">0</h3>
                    <p class="text-xs text-gray" style="margin-top:4px;">بۈگۈنكى زاكاز</p>
                </div>
                <div class="luxury-card" style="padding:16px;">
                    <div class="icon-container" style="background:linear-gradient(135deg,rgba(139,92,246,0.3),rgba(109,40,217,0.2));margin-bottom:12px;">💰</div>
                    <h3 class="stat-number text-purple" id="todayProfit">$0</h3>
                    <p class="text-xs text-gray" style="margin-top:4px;">ساپ پايدا</p>
                </div>
            </div>
            
            <div class="card mb-4">
                <h3 class="font-bold mb-3">📅 ئايلىق ستاتىستىكا</h3>
                <div class="grid grid-3 gap-2">
                    <div class="text-center p-3 bg-green rounded-lg">
                        <p class="text-lg font-bold text-green" id="monthRevenue">$0</p>
                        <p class="text-xs text-gray">كىرىم</p>
                    </div>
                    <div class="text-center p-3 bg-red rounded-lg">
                        <p class="text-lg font-bold text-red" id="monthExpense">$0</p>
                        <p class="text-xs text-gray">چىقىم</p>
                    </div>
                    <div class="text-center p-3 bg-purple rounded-lg">
                        <p class="text-lg font-bold text-purple" id="monthProfit">$0</p>
                        <p class="text-xs text-gray">پايدا</p>
                    </div>
                </div>
            </div>
            
            <div class="luxury-card mb-4" style="padding:16px;">
                <h3 class="font-bold mb-3" style="display:flex;align-items:center;gap:8px;"><span class="floating-icon-slow" style="font-size:20px;">⚡</span> تېز مەشغۇلات</h3>
                <div class="grid grid-4 gap-3">
                    <button onclick="showPage('pos')" class="btn btn-3d" style="flex-direction:column;padding:16px 12px;background:linear-gradient(135deg,rgba(16,185,129,0.3),rgba(5,150,105,0.2));color:#10b981;border-radius:16px;border:1px solid rgba(16,185,129,0.3);">
                        <span style="font-size:28px;" class="floating-icon-slow">💰</span>
                        <span class="text-xs" style="margin-top:8px;">سېتىش</span>
                    </button>
                    <button onclick="openExpenseModal()" class="btn btn-3d" style="flex-direction:column;padding:16px 12px;background:linear-gradient(135deg,rgba(239,68,68,0.3),rgba(185,28,28,0.2));color:#ef4444;border-radius:16px;border:1px solid rgba(239,68,68,0.3);">
                        <span style="font-size:28px;" class="floating-icon-slow">💸</span>
                        <span class="text-xs" style="margin-top:8px;">چىقىم</span>
                    </button>
                    <button onclick="showPage('rooms')" class="btn btn-3d" style="flex-direction:column;padding:16px 12px;background:linear-gradient(135deg,rgba(139,92,246,0.3),rgba(109,40,217,0.2));color:#8b5cf6;border-radius:16px;border:1px solid rgba(139,92,246,0.3);">
                        <span style="font-size:28px;" class="floating-icon-slow">🚪</span>
                        <span class="text-xs" style="margin-top:8px;">خانا</span>
                    </button>
                    <button onclick="showPage('reports')" class="btn btn-3d" style="flex-direction:column;padding:16px 12px;background:linear-gradient(135deg,rgba(59,130,246,0.3),rgba(29,78,216,0.2));color:#3b82f6;border-radius:16px;border:1px solid rgba(59,130,246,0.3);">
                        <span style="font-size:28px;" class="floating-icon-slow">📊</span>
                        <span class="text-xs" style="margin-top:8px;">دوكلات</span>
                    </button>
                </div>
            </div>
            
            <!-- ئاممىباب تائاملار -->
            <div class="card mb-4">
                <div class="flex items-center justify-between mb-3">
                    <h3 class="font-bold">🔥 ئاممىباب تائاملار</h3>
                    <span class="text-xs text-gray">ئەڭ كۆپ سېتىلغانلار</span>
                </div>
                <div id="popularFoods" class="grid grid-3 gap-2">
                    <p class="text-center text-gray p-4" style="grid-column:span 3;">سانلىق مەلۇمات يوق</p>
                </div>
            </div>
            
            <div class="card">
                <div class="flex items-center justify-between mb-3">
                    <h3 class="font-bold">🕐 يېقىنقى زاكازلار</h3>
                    <button onclick="showPage('orders')" class="text-sm text-blue" style="background:none;border:none;cursor:pointer;">ھەممىسى</button>
                </div>
                <div id="recentOrders">
                    <p class="text-center text-gray p-4">زاكاز يوق</p>
                </div>
            </div>
        </div>

        <!-- POS Page -->
        <div class="page" id="page-pos">
            <!-- Step 1: ئورۇن تاللاش -->
            <div id="posStep1">
                <h2 class="text-xl font-bold mb-4 text-center">📍 ئورۇن تاللاڭ</h2>
                
                <div class="card mb-4" onclick="selectLocation('takeaway')" style="cursor:pointer;border:2px solid rgba(249,115,22,0.5);background:rgba(249,115,22,0.1);">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">🛍️</span>
                        <div class="flex-1">
                            <h3 class="font-bold">ئېلىپ كېتىش</h3>
                            <p class="text-xs text-gray">تاكا سودا</p>
                        </div>
                        <span class="text-orange">←</span>
                    </div>
                </div>
                
                <h3 class="font-bold mb-3">🪑 ئۈستەللەر</h3>
                <div class="grid grid-4 gap-2 mb-4" id="posTablesGrid"></div>
                
                <h3 class="font-bold mb-3">🚪 ئايرىمخانىلار</h3>
                <div class="grid grid-2 gap-3" id="posRoomsGrid"></div>
            </div>
            
            <!-- Step 2: تائام تاللاش -->
            <div id="posStep2" style="display:none;">
                <div class="flex items-center gap-3 mb-3">
                    <button onclick="backToStep1()" class="btn" style="padding:8px 12px;background:rgba(255,255,255,0.1);">→</button>
                    <div class="flex-1 card p-2">
                        <span id="selectedLocationIcon">📍</span>
                        <span class="font-bold" id="selectedLocationName">ئورۇن</span>
                    </div>
                </div>
                
                <div class="card p-3 mb-3">
                    <div class="grid grid-2 gap-2">
                        <div>
                            <label class="text-xs text-orange mb-1" style="display:block;">👤 خىزمەتچى</label>
                            <select id="posEmployee"></select>
                        </div>
                        <div>
                            <label class="text-xs text-gray mb-1" style="display:block;">🧑 خېرىدار</label>
                            <input type="text" id="posCustomer" placeholder="ئىختىيارى...">
                        </div>
                    </div>
                </div>
                
                <div class="filter-scroll mb-3" id="posCategoryFilter"></div>
                
                <div class="grid grid-3 gap-2" id="posMenuGrid" style="max-height:35vh;overflow-y:auto;"></div>
            </div>
            
            <!-- Step 3: سېۋەت -->
            <div id="posStep3" class="cart-fixed" style="display:none;">
                <div class="flex items-center justify-between mb-2">
                    <div class="flex items-center gap-2">
                        <span>🛒</span>
                        <span class="font-bold text-green" id="cartCount">0</span>
                        <span class="text-sm text-gray">تائام</span>
                    </div>
                    <span class="text-lg font-bold text-green" id="cartTotal">$0.00</span>
                </div>
                
                <div id="cartItems" style="max-height:15vh;overflow-y:auto;"></div>
                
                <div class="grid grid-3 gap-2 mb-3">
                    <button onclick="selectPayment('usd')" class="btn pay-btn active" style="background:rgba(16,185,129,0.3);border:2px solid #10b981;">💵 دوللار</button>
                    <button onclick="selectPayment('try')" class="btn pay-btn" style="background:rgba(255,255,255,0.05);border:2px solid transparent;">💴 لىرا</button>
                    <button onclick="selectPayment('cash')" class="btn pay-btn" style="background:rgba(255,255,255,0.05);border:2px solid transparent;">💰 شامكاش</button>
                </div>
                
                <div id="liraConvert" style="display:none;background:rgba(245,158,11,0.1);padding:8px;border-radius:8px;text-align:center;margin-bottom:8px;">
                    <span class="text-sm text-gray">لىرا بىلەن:</span>
                    <span class="font-bold text-yellow" id="cartTotalTRY">₺0</span>
                </div>
                
                <div class="grid grid-3 gap-2">
                    <button onclick="clearCart()" class="btn btn-danger">🗑️ ئۆچۈر</button>
                    <button onclick="completeSale()" class="btn btn-success">✅ سېتىش</button>
                    <button onclick="printReceipt()" class="btn" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🖨️ تالون</button>
                </div>
            </div>
        </div>

        <!-- Menu Page -->
        <div class="page" id="page-menu">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">📖 تائام تىزىملىكى</h2>
                <button onclick="openMenuModal()" class="btn btn-primary" style="padding:8px 16px;">➕ قوش</button>
            </div>
            
            <!-- ئىزدەش -->
            <div class="mb-4">
                <input type="text" id="menuSearch" placeholder="🔍 تائام ئىزدەش..." oninput="searchMenu()" class="input-luxury">
            </div>
            
            <div class="filter-scroll mb-4" id="menuCategoryFilter"></div>
            
            <!-- ستاتىستىكا -->
            <div class="flex gap-3 mb-4 text-xs">
                <span class="bg-blue rounded-lg p-2">جەمئىي: <span id="menuCount" class="font-bold text-blue">0</span></span>
                <span class="bg-green rounded-lg p-2">بار: <span id="menuAvailableCount" class="font-bold text-green">0</span></span>
                <span class="bg-red rounded-lg p-2">يوق: <span id="menuUnavailableCount" class="font-bold text-red">0</span></span>
            </div>
            
            <div id="menuList"></div>
        </div>

        <!-- Orders Page -->
        <div class="page" id="page-orders">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">📋 زاكازلار</h2>
                <div id="newOrderBadge" style="display:none;background:#ef4444;color:white;padding:4px 12px;border-radius:20px;font-size:12px;animation:pulse 1s infinite;">
                    🔔 يېڭى زاكاز!
                </div>
            </div>
            
            <!-- ئىزدەش ۋە سۈزگۈچ -->
            <div class="card mb-4 p-3">
                <div class="grid grid-2 gap-2 mb-3">
                    <div>
                        <input type="text" id="orderSearch" placeholder="🔍 زاكاز ئىزدەش..." oninput="searchOrders()" class="input-luxury" style="font-size:14px;">
                    </div>
                    <div>
                        <input type="date" id="orderDateFilter" onchange="filterOrdersByDate()" style="font-size:14px;">
                    </div>
                </div>
                <div class="flex gap-2 items-center justify-between">
                    <div class="text-xs text-gray">
                        جەمئىي: <span id="ordersCount" class="text-green font-bold">0</span> زاكاز
                    </div>
                    <button onclick="clearOrderFilters()" class="btn text-xs" style="background:rgba(255,255,255,0.1);padding:6px 12px;">🔄 تازىلاش</button>
                </div>
            </div>
            
            <div class="filter-scroll mb-4">
                <button class="filter-btn active" onclick="filterOrders('all')">ھەممىسى</button>
                <button class="filter-btn" onclick="filterOrders('pending')">⏳ ساقلاۋاتقان</button>
                <button class="filter-btn" onclick="filterOrders('preparing')">🔥 تەييارلاۋاتقان</button>
                <button class="filter-btn" onclick="filterOrders('completed')">✅ تامام</button>
                <button class="filter-btn" onclick="filterOrders('customer')" style="background:rgba(16,185,129,0.2);color:#10b981;">📱 خېرىدار زاكازلىرى</button>
            </div>
            <div id="ordersList"></div>
        </div>

        <!-- Tables Page -->
        <div class="page" id="page-tables">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">🪑 ئۈستەللەر</h2>
                <button onclick="openTableModal()" class="btn btn-primary" style="padding:8px 16px;">➕ قوش</button>
            </div>
            <div class="flex gap-4 mb-4">
                <div class="flex items-center gap-2"><span style="width:12px;height:12px;background:#10b981;border-radius:50%;"></span><span class="text-sm">بوش</span></div>
                <div class="flex items-center gap-2"><span style="width:12px;height:12px;background:#ef4444;border-radius:50%;"></span><span class="text-sm">ئىگىلەنگەن</span></div>
            </div>
            <div class="grid grid-3 gap-3" id="tablesList"></div>
        </div>

        <!-- Rooms Page -->
        <div class="page" id="page-rooms">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">🚪 ئايرىمخانىلار</h2>
                <button onclick="openRoomModal()" class="btn btn-primary" style="padding:8px 16px;">➕ قوش</button>
            </div>
            <div id="roomsList"></div>
        </div>

        <!-- Employees Page -->
        <div class="page" id="page-employees">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">👨‍🍳 خىزمەتچىلەر</h2>
                <button onclick="openEmployeeModal()" class="btn btn-primary" style="padding:8px 16px;">➕ قوش</button>
            </div>
            <div id="employeesList"></div>
        </div>

        <!-- Expenses Page -->
        <div class="page" id="page-expenses">
            <div class="flex items-center justify-between mb-4">
                <h2 class="text-xl font-bold">💸 چىقىملار</h2>
                <button onclick="openExpenseModal()" class="btn btn-primary" style="padding:8px 16px;">➕ قوش</button>
            </div>
            <div class="filter-scroll mb-4" id="expenseFilter"></div>
            <div id="expensesList"></div>
        </div>

        <!-- Cashbox Page -->
        <div class="page" id="page-cashbox">
            <h2 class="text-xl font-bold mb-4">🏦 ساندۇق باشقۇرۇش</h2>
            
            <div class="card mb-3" style="border:2px solid rgba(16,185,129,0.5);">
                <div class="flex items-center justify-between mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">💵</span>
                        <div>
                            <h3 class="font-bold">دوللار ساندۇقى</h3>
                            <p class="text-xs text-gray">USD</p>
                        </div>
                    </div>
                    <span class="text-2xl font-bold text-green" id="usdBalance">$0.00</span>
                </div>
                <div class="grid grid-3 gap-2">
                    <button onclick="openCashboxModal('usd','in')" class="btn text-sm" style="background:rgba(16,185,129,0.2);color:#10b981;">➕ كىرىم</button>
                    <button onclick="openCashboxModal('usd','out')" class="btn text-sm" style="background:rgba(239,68,68,0.2);color:#ef4444;">➖ چىقىم</button>
                    <button onclick="openTransferModal('usd')" class="btn text-sm" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🔄 يۆتكەش</button>
                </div>
            </div>
            
            <div class="card mb-3" style="border:2px solid rgba(245,158,11,0.5);">
                <div class="flex items-center justify-between mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">💴</span>
                        <div>
                            <h3 class="font-bold">لىرا ساندۇقى</h3>
                            <p class="text-xs text-gray">TRY</p>
                        </div>
                    </div>
                    <span class="text-2xl font-bold text-yellow" id="tryBalance">₺0.00</span>
                </div>
                <div class="grid grid-3 gap-2">
                    <button onclick="openCashboxModal('try','in')" class="btn text-sm" style="background:rgba(16,185,129,0.2);color:#10b981;">➕ كىرىم</button>
                    <button onclick="openCashboxModal('try','out')" class="btn text-sm" style="background:rgba(239,68,68,0.2);color:#ef4444;">➖ چىقىم</button>
                    <button onclick="openTransferModal('try')" class="btn text-sm" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🔄 يۆتكەش</button>
                </div>
            </div>
            
            <div class="card mb-3" style="border:2px solid rgba(139,92,246,0.5);">
                <div class="flex items-center justify-between mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">💰</span>
                        <div>
                            <h3 class="font-bold">شامكاش ساندۇقى</h3>
                            <p class="text-xs text-gray">Cash</p>
                        </div>
                    </div>
                    <span class="text-2xl font-bold text-purple" id="cashBalance">$0.00</span>
                </div>
                <div class="grid grid-3 gap-2">
                    <button onclick="openCashboxModal('cash','in')" class="btn text-sm" style="background:rgba(16,185,129,0.2);color:#10b981;">➕ كىرىم</button>
                    <button onclick="openCashboxModal('cash','out')" class="btn text-sm" style="background:rgba(239,68,68,0.2);color:#ef4444;">➖ چىقىم</button>
                    <button onclick="openTransferModal('cash')" class="btn text-sm" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🔄 يۆتكەش</button>
                </div>
            </div>
            
            <div class="card mb-4" style="background:linear-gradient(135deg,rgba(99,102,241,0.2),rgba(139,92,246,0.2));">
                <h3 class="font-bold mb-2">📊 جەمئىي قالدۇق (USD)</h3>
                <p class="text-3xl font-bold text-center" id="totalBalance" style="background:linear-gradient(135deg,#6366f1,#8b5cf6);-webkit-background-clip:text;-webkit-text-fill-color:transparent;">$0.00</p>
            </div>
            
            <div class="card">
                <div class="flex items-center justify-between mb-3">
                    <h3 class="font-bold">📜 مەشغۇلات تارىخى</h3>
                    <button onclick="exportTransactions()" class="btn text-xs" style="background:rgba(16,185,129,0.2);color:#10b981;">📥 JSON</button>
                </div>
                <div id="transactionsList" style="max-height:300px;overflow-y:auto;"></div>
            </div>
        </div>

        <!-- Reports Page -->
        <div class="page" id="page-reports">
            <h2 class="text-xl font-bold mb-4">📈 دوكلاتلار</h2>
            <div class="card mb-4">
                <div class="grid grid-2 gap-3 mb-3">
                    <div>
                        <label class="text-xs text-gray mb-1" style="display:block;">باشلىنىش</label>
                        <input type="date" id="reportFrom">
                    </div>
                    <div>
                        <label class="text-xs text-gray mb-1" style="display:block;">ئاياغلىشىش</label>
                        <input type="date" id="reportTo">
                    </div>
                </div>
                <button onclick="generateReport()" class="btn btn-primary" style="width:100%;">📊 دوكلات ھاسىللاش</button>
            </div>
            <div class="grid gap-3">
                <div class="card flex items-center gap-4">
                    <span style="font-size:32px;">📈</span>
                    <div class="flex-1">
                        <p class="text-gray text-sm">جەمئىي كىرىم</p>
                        <h3 class="text-2xl font-bold text-green" id="reportRevenue">$0</h3>
                    </div>
                </div>
                <div class="card flex items-center gap-4">
                    <span style="font-size:32px;">📉</span>
                    <div class="flex-1">
                        <p class="text-gray text-sm">جەمئىي چىقىم</p>
                        <h3 class="text-2xl font-bold text-red" id="reportExpense">$0</h3>
                    </div>
                </div>
                <div class="card flex items-center gap-4">
                    <span style="font-size:32px;">💰</span>
                    <div class="flex-1">
                        <p class="text-gray text-sm">ساپ پايدا</p>
                        <h3 class="text-2xl font-bold text-purple" id="reportProfit">$0</h3>
                    </div>
                </div>
            </div>
            <button onclick="exportReport()" class="btn mt-4" style="width:100%;background:rgba(16,185,129,0.2);color:#10b981;">📥 JSON غا چىقىرىش</button>
        </div>

        <!-- Customer Order Page -->
        <div class="page" id="page-customer">
            <button id="exitCustomerBtn" onclick="exitCustomerMode()" style="position:fixed;top:16px;left:16px;background:rgba(239,68,68,0.2);border:1px solid rgba(239,68,68,0.5);color:#ef4444;padding:8px 12px;border-radius:10px;font-size:12px;cursor:pointer;z-index:100;">
                ← باشقۇرۇش
            </button>
            
            <!-- 1-قەدەم: ئورۇن تاللاش -->
            <div id="customerStep1">
                <div class="text-center mb-4" style="padding-top:20px;">
                    <div style="font-size:60px;" class="floating-icon">🍽️</div>
                    <h2 class="text-2xl font-bold mt-2" id="customerRestaurantName">رېستوران</h2>
                    <p class="text-gray text-sm">تائام زاكاز قىلىش سىستىمىسى</p>
                </div>
                
                <h2 class="text-xl font-bold mb-4 text-center">📍 ئورۇن تاللاڭ</h2>
                
                <!-- ئېلىپ كېتىش -->
                <div class="card mb-4" onclick="selectCustomerLocation('takeaway')" style="cursor:pointer;border:2px solid rgba(249,115,22,0.5);background:linear-gradient(135deg,rgba(249,115,22,0.2),rgba(234,88,12,0.1));">
                    <div class="flex items-center gap-3">
                        <span style="font-size:40px;" class="floating-icon-slow">🛍️</span>
                        <div class="flex-1">
                            <h3 class="font-bold text-lg">ئېلىپ كېتىش</h3>
                            <p class="text-xs text-gray">تائامنى ئېلىپ كېتىمەن</p>
                        </div>
                        <span class="text-orange text-2xl">←</span>
                    </div>
                </div>
                
                <!-- ئۈستەللەر -->
                <h3 class="font-bold mb-3">🪑 ئۈستەل تاللاش</h3>
                <div class="grid grid-4 gap-2 mb-4" id="customerTablesGrid"></div>
                
                <!-- ئايرىمخانىلار -->
                <h3 class="font-bold mb-3">🚪 ئايرىمخانا تاللاش</h3>
                <div class="grid grid-2 gap-3" id="customerRoomsGrid"></div>
            </div>
            
            <!-- 2-قەدەم: تائام تاللاش -->
            <div id="customerStep2" style="display:none;height:100vh;display:flex;flex-direction:column;">
                <div class="flex items-center gap-3 mb-2" style="padding-top:50px;">
                    <button onclick="backToCustomerStep1()" class="btn btn-3d" style="padding:8px 12px;background:rgba(255,255,255,0.1);border-radius:12px;">
                        <span style="font-size:16px;">→</span>
                    </button>
                    <div class="flex-1 card p-2" style="background:linear-gradient(135deg,rgba(99,102,241,0.2),rgba(139,92,246,0.2));">
                        <span id="customerSelectedIcon" style="font-size:20px;">📍</span>
                        <span class="font-bold text-sm" id="customerSelectedName">ئورۇن</span>
                    </div>
                    <div class="card p-2" style="background:rgba(255,255,255,0.05);">
                        <input type="text" id="customerName" placeholder="👤 ئىسمىڭىز..." style="background:transparent;border:none;outline:none;color:white;width:100px;font-size:12px;">
                    </div>
                </div>
                
                <div class="filter-scroll mb-2" id="customerCategoryFilter"></div>
                
                <div class="grid grid-3 gap-2" id="customerMenuGrid" style="flex:1;overflow-y:auto;padding-bottom:200px;-webkit-overflow-scrolling:touch;"></div>
            </div>
            
            <!-- 3-قەدەم: سېۋەت -->
            <div id="customerStep3" style="display:none;position:fixed;bottom:0;left:0;right:0;background:rgba(15,23,42,0.98);backdrop-filter:blur(10px);border-top:1px solid rgba(255,255,255,0.1);padding:12px;z-index:90;max-height:50vh;overflow-y:auto;">
                <div class="flex items-center justify-between mb-3">
                    <div class="flex items-center gap-2">
                        <span style="font-size:24px;" class="floating-icon-slow">🛒</span>
                        <span class="font-bold">سېۋىتىم</span>
                        <span class="bg-green text-white rounded-full" style="padding:2px 10px;font-size:12px;" id="customerCartCount">0</span>
                    </div>
                    <span class="text-xl font-bold text-green" id="customerCartTotal">$0.00</span>
                </div>
                
                <div id="customerCartItems" style="max-height:15vh;overflow-y:auto;margin-bottom:12px;"></div>
                
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">📝 ئالاھىدە تەلەپ (ئىختىيارى)</label>
                    <input type="text" id="customerNote" placeholder="مەسىلەن: ئاچچىق ئاز بولسۇن..." class="input-luxury">
                </div>
                
                <div class="grid grid-2 gap-3">
                    <button onclick="clearCustomerCart()" class="btn btn-danger btn-3d">🗑️ تازىلاش</button>
                    <button onclick="submitCustomerOrder()" class="btn btn-success btn-3d glow-effect" style="background:linear-gradient(135deg,#10b981,#059669);">📤 زاكاز يوللاش</button>
                </div>
            </div>
        </div>

        <!-- Settings Page -->
        <div class="page" id="page-settings">
            <h2 class="text-xl font-bold mb-4">⚙️ تەڭشەكلەر</h2>
            <div class="card mb-4">
                <h3 class="font-bold mb-3">🏪 رېستوران ئۇچۇرى</h3>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">نامى</label>
                    <input type="text" id="settingName" placeholder="رېستوران نامى">
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">تېلېفون</label>
                    <input type="tel" id="settingPhone" placeholder="تېلېفون">
                </div>
                <div>
                    <label class="text-xs text-gray mb-1" style="display:block;">ئادرېس</label>
                    <textarea id="settingAddress" placeholder="ئادرېس" style="height:60px;"></textarea>
                </div>
            </div>
            
            <div class="card mb-4">
                <h3 class="font-bold mb-3">💱 تېگىشىش نىسبىتى</h3>
                <div class="flex items-center gap-3 p-3 rounded-lg" style="background:rgba(255,255,255,0.05);">
                    <span class="text-green font-bold">1 $</span>
                    <span>=</span>
                    <input type="number" id="exchangeRate" value="34.50" step="0.01" style="width:100px;text-align:center;">
                    <span class="text-yellow font-bold">₺</span>
                </div>
            </div>
            
            <div class="card mb-4">
                <h3 class="font-bold mb-3">🔐 شىفىرە باشقۇرۇش</h3>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">ھازىرقى شىفىرە</label>
                    <input type="password" id="currentPin" placeholder="ھازىرقى 6 خانىلىق شىفىرە" maxlength="6" style="letter-spacing:8px;text-align:center;">
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">يېڭى شىفىرە</label>
                    <input type="password" id="newPin" placeholder="يېڭى 6 خانىلىق شىفىرە" maxlength="6" style="letter-spacing:8px;text-align:center;">
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">يېڭى شىفىرىنى تەكرارلاڭ</label>
                    <input type="password" id="confirmPin" placeholder="يېڭى شىفىرىنى تەكرارلاڭ" maxlength="6" style="letter-spacing:8px;text-align:center;">
                </div>
                <button onclick="changePin()" class="btn" style="width:100%;background:rgba(139,92,246,0.2);color:#8b5cf6;">🔑 شىفىرە ئۆزگەرتىش</button>
                
                <div class="mt-4 p-3 rounded-lg" style="background:rgba(59,130,246,0.1);border:1px solid rgba(59,130,246,0.3);">
                    <h4 class="font-bold text-sm text-blue mb-2">⏱️ ئاپتوماتىك قۇلۇپلاش</h4>
                    <div class="flex items-center gap-3">
                        <select id="autoLockTime" onchange="saveAutoLockTime()" style="flex:1;">
                            <option value="5">5 مىنۇت</option>
                            <option value="10">10 مىنۇت</option>
                            <option value="15">15 مىنۇت</option>
                            <option value="30" selected>30 مىنۇت</option>
                            <option value="60">1 سائەت</option>
                            <option value="0">ئۆچۈرۈلگەن</option>
                        </select>
                    </div>
                    <p class="text-xs text-gray mt-2">سېستىما ئىشلىتىلمىسە ئاپتوماتىك قۇلۇپلىنىدۇ</p>
                </div>
            </div>
            
            <div class="card mb-4">
                <h3 class="font-bold mb-3">💾 سانلىق مەلۇمات (IndexedDB - 100MB+)</h3>
                <div class="p-3 mb-3 rounded-lg" style="background:rgba(16,185,129,0.1);border:1px solid rgba(16,185,129,0.3);">
                    <div class="flex items-center gap-2 mb-2">
                        <span>💽</span>
                        <span class="text-sm">ساقلاش بوشلۇقى:</span>
                        <span class="font-bold text-green" id="storageInfoDetail">ھېسابلاۋاتىدۇ...</span>
                    </div>
                    <div style="height:8px;background:rgba(255,255,255,0.1);border-radius:4px;overflow:hidden;">
                        <div id="storageBarDetail" style="height:100%;background:linear-gradient(90deg,#10b981,#059669);width:0%;transition:width 0.3s;"></div>
                    </div>
                </div>
                <button onclick="backupData()" class="btn mb-2" style="width:100%;background:rgba(16,185,129,0.2);color:#10b981;">📥 زاپاسلاش (JSON)</button>
                <label class="btn mb-2" style="width:100%;background:rgba(59,130,246,0.2);color:#3b82f6;cursor:pointer;">
                    📤 ئەسلىگە كەلتۈرۈش
                    <input type="file" accept=".json" onchange="restoreData(this)" style="display:none;">
                </label>
                <button onclick="archiveOldOrders(30)" class="btn mb-2" style="width:100%;background:rgba(245,158,11,0.2);color:#f59e0b;">📦 30 كۈندىن كونا زاكازلارنى ئارخىپلاش</button>
                <button onclick="clearAllData()" class="btn btn-danger" style="width:100%;">🗑️ ھەممىنى ئۆچۈرۈش</button>
            </div>
            
            <button onclick="saveSettings()" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
        </div>
    </main>

    <!-- Bottom Navigation -->
    <nav class="bottom-nav" style="background:linear-gradient(180deg,rgba(15,23,42,0.98),rgba(15,23,42,1));box-shadow:0 -10px 40px rgba(0,0,0,0.3);">
        <button class="nav-item-luxury active" onclick="showPage('dashboard')"><span>🏠</span><span>باش بەت</span></button>
        <button class="nav-item-luxury" onclick="showPage('pos')"><span>💰</span><span>سېتىش</span></button>
        <button class="nav-item-luxury" onclick="showPage('orders')"><span>📋</span><span>زاكازلار</span></button>
        <button class="nav-item-luxury" onclick="showPage('reports')"><span>📊</span><span>دوكلات</span></button>
    </nav>

    <!-- Modals -->
    <div class="modal" id="menuModal">
        <div class="modal-overlay" onclick="closeModal('menuModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">📖 تائام قوشۇش</h3>
            <form id="menuForm">
                <input type="hidden" id="menuId">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">تائام نامى</label>
                    <input type="text" id="menuName" required>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">تۈرى</label>
                    <select id="menuCategory"></select>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">باھاسى ($)</label>
                    <input type="number" id="menuPrice" step="0.01" required>
                </div>
                <div class="mb-4 flex items-center gap-3">
                    <input type="checkbox" id="menuAvailable" checked style="width:20px;height:20px;">
                    <label for="menuAvailable">بار</label>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="tableModal">
        <div class="modal-overlay" onclick="closeModal('tableModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">🪑 ئۈستەل قوشۇش</h3>
            <form id="tableForm">
                <input type="hidden" id="tableId">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">نومۇرى</label>
                    <input type="number" id="tableNumber" required>
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">ئورۇن سانى</label>
                    <input type="number" id="tableCapacity" value="4" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="roomModal">
        <div class="modal-overlay" onclick="closeModal('roomModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">🚪 ئايرىمخانا قوشۇش</h3>
            <form id="roomForm">
                <input type="hidden" id="roomId">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">نامى</label>
                    <input type="text" id="roomName" required placeholder="مەسىلەن: VIP-1">
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">ئورۇن سانى</label>
                    <input type="number" id="roomCapacity" value="8" required>
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">سائەتلىك باھاسى ($)</label>
                    <input type="number" id="roomPrice" step="0.01" value="0">
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="employeeModal">
        <div class="modal-overlay" onclick="closeModal('employeeModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">👨‍🍳 خىزمەتچى قوشۇش</h3>
            <form id="employeeForm">
                <input type="hidden" id="employeeId">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">ئىسمى</label>
                    <input type="text" id="employeeName" required>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">تېلېفون</label>
                    <input type="tel" id="employeePhone">
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">ۋەزىپىسى</label>
                    <select id="employeeRole">
                        <option value="waiter">ئوفىتسىيانت</option>
                        <option value="chef">ئاشپەز</option>
                        <option value="cashier">كاسسىر</option>
                        <option value="manager">مۇدىر</option>
                    </select>
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">ئايلىق مائاش ($)</label>
                    <input type="number" id="employeeSalary" step="0.01" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="expenseModal">
        <div class="modal-overlay" onclick="closeModal('expenseModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">💸 چىقىم قوشۇش</h3>
            <form id="expenseForm">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">تۈرى</label>
                    <select id="expenseCategory">
                        <option value="vegetable">🥬 سەي كۆكتات</option>
                        <option value="meat">🥩 گۆش</option>
                        <option value="coal">�ite كۆمۈر</option>
                        <option value="utility">💡 توك/سۇ</option>
                        <option value="gas">🔥 گاز</option>
                        <option value="flour">🌾 ئۇن</option>
                        <option value="oil">🛢️ ياغ</option>
                        <option value="rice">🍚 گۈرۈچ</option>
                        <option value="salary">💰 مائاش</option>
                        <option value="other">📦 باشقا</option>
                    </select>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">چۈشەندۈرۈش</label>
                    <input type="text" id="expenseDesc">
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">سوممىسى ($)</label>
                    <input type="number" id="expenseAmount" step="0.01" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">💾 ساقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="cashboxModal">
        <div class="modal-overlay" onclick="closeModal('cashboxModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4" id="cashboxModalTitle">💰 ساندۇق مەشغۇلاتى</h3>
            <form id="cashboxForm">
                <input type="hidden" id="cashboxType">
                <input type="hidden" id="cashboxAction">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">سوممىسى</label>
                    <input type="number" id="cashboxAmount" step="0.01" required>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">چۈشەندۈرۈش</label>
                    <input type="text" id="cashboxNote">
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">مەسئۇل</label>
                    <select id="cashboxPerson"></select>
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">✅ تەستىقلاش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="transferModal">
        <div class="modal-overlay" onclick="closeModal('transferModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <h3 class="text-lg font-bold mb-4">🔄 پۇل يۆتكەش</h3>
            <form id="transferForm">
                <input type="hidden" id="transferFrom">
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">قايسى ساندۇقتىن</label>
                    <div class="p-3 rounded-lg" style="background:rgba(255,255,255,0.05);" id="transferFromDisplay"></div>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">قايسى ساندۇققا</label>
                    <select id="transferTo"></select>
                </div>
                <div class="mb-3">
                    <label class="text-xs text-gray mb-1" style="display:block;">سوممىسى</label>
                    <input type="number" id="transferAmount" step="0.01" required>
                </div>
                <div class="mb-3 p-3 rounded-lg" style="background:rgba(99,102,241,0.1);">
                    <p class="text-xs text-gray">نىسبەت: <span id="transferRate">1$ = 34.50₺</span></p>
                    <p class="text-green font-bold" id="transferResult"></p>
                </div>
                <div class="mb-4">
                    <label class="text-xs text-gray mb-1" style="display:block;">چۈشەندۈرۈش</label>
                    <input type="text" id="transferNote">
                </div>
                <button type="submit" class="btn btn-primary" style="width:100%;">🔄 يۆتكەش</button>
            </form>
        </div>
    </div>

    <div class="modal" id="receiptModal">
        <div class="modal-overlay" onclick="closeModal('receiptModal')"></div>
        <div class="modal-content">
            <div class="modal-handle"></div>
            <div class="receipt" id="receiptContent"></div>
            <div class="grid grid-2 gap-3 mt-4">
                <button onclick="closeModal('receiptModal')" class="btn" style="background:rgba(255,255,255,0.1);">✕ تاقاش</button>
                <button onclick="printReceiptContent()" class="btn btn-primary">🖨️ بېسىش</button>
            </div>
        </div>
    </div>

    <script>
        // ==================== شىفىرە ۋە بىخەتەرلىك سىستىمىسى ====================
        let currentPinInput = '';
        let loginAttempts = 0;
        let maxAttempts = 5;
        let lockoutTime = 0;
        let sessionTimeout = null;
        let sessionWarningTimeout = null;
        let autoLockMinutes = 30;
        let lastActivity = Date.now();
        
        // شىفىرىنى شىفىرلەش (SHA-256)
        async function hashPin(pin) {
            const encoder = new TextEncoder();
            const data = encoder.encode(pin + 'RestaurantSalt2024');
            const hashBuffer = await crypto.subtle.digest('SHA-256', data);
            const hashArray = Array.from(new Uint8Array(hashBuffer));
            return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
        }
        
        // شىفىرە كىرگۈزۈش
        function enterPin(num) {
            if (currentPinInput.length >= 6) return;
            if (lockoutTime > Date.now()) {
                const remaining = Math.ceil((lockoutTime - Date.now()) / 1000);
                document.getElementById('loginError').textContent = `${remaining} سېكۇنت ساقلاڭ`;
                return;
            }
            
            currentPinInput += num;
            updatePinDisplay();
            
            // 6 خانە تولغاندا ئاپتوماتىك تەكشۈرۈش
            if (currentPinInput.length === 6) {
                setTimeout(() => submitPin(), 200);
            }
        }
        
        // شىفىرە كۆرسىتىش يېڭىلاش
        function updatePinDisplay() {
            const dots = document.querySelectorAll('.pin-dot');
            dots.forEach((dot, index) => {
                dot.classList.remove('filled', 'error');
                if (index < currentPinInput.length) {
                    dot.classList.add('filled');
                }
            });
        }
        
        // شىفىرە تازىلاش
        function clearPin() {
            if (currentPinInput.length > 0) {
                currentPinInput = currentPinInput.slice(0, -1);
                updatePinDisplay();
            }
        }
        
        // شىفىرە يوللاش
        async function submitPin() {
            if (currentPinInput.length !== 6) {
                document.getElementById('loginError').textContent = '6 خانىلىق شىفىرە كىرگۈزۈڭ';
                return;
            }
            
            // قۇلۇپلانغان مۇ؟
            if (lockoutTime > Date.now()) {
                const remaining = Math.ceil((lockoutTime - Date.now()) / 1000);
                document.getElementById('loginError').textContent = `${remaining} سېكۇنت ساقلاڭ`;
                currentPinInput = '';
                updatePinDisplay();
                return;
            }
            
            const hashedInput = await hashPin(currentPinInput);
            const savedPin = localStorage.getItem('systemPin');
            
            // بىرىنچى قېتىم - شىفىرە تەڭشەش
            if (!savedPin) {
                localStorage.setItem('systemPin', hashedInput);
                loginSuccess();
                toast('شىفىرە تەڭشەلدى! بۇ شىفىرىنى ئەستە ساقلاڭ', '🔐');
                return;
            }
            
            // شىفىرە توغرىمۇ؟
            if (hashedInput === savedPin) {
                loginAttempts = 0;
                loginSuccess();
            } else {
                loginAttempts++;
                showPinError();
                
                if (loginAttempts >= maxAttempts) {
                    lockoutTime = Date.now() + (60000 * loginAttempts); // ھەر قېتىم 1 مىنۇت ئاشىدۇ
                    document.getElementById('loginError').textContent = `بەك كۆپ خاتا! ${loginAttempts} مىنۇت ساقلاڭ`;
                } else {
                    document.getElementById('loginError').textContent = `خاتا شىفىرە! ${maxAttempts - loginAttempts} قېتىم پۇرسەت قالدى`;
                }
            }
            
            currentPinInput = '';
            setTimeout(() => updatePinDisplay(), 500);
        }
        
        // شىفىرە خاتا كۆرسىتىش
        function showPinError() {
            const dots = document.querySelectorAll('.pin-dot');
            dots.forEach(dot => dot.classList.add('error'));
        }
        
        // كىرىش مۇۋەپپەقىيەتلىك
        function loginSuccess() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('lockBtn').style.display = 'block';
            lastActivity = Date.now();
            startSessionTimer();
            
            // يۈكلەش ئېكرانى
            document.getElementById('loadingScreen').style.display = 'flex';
            setTimeout(() => {
                document.getElementById('loadingScreen').style.display = 'none';
                init();
            }, 800);
        }
        
        // سېستىمىنى قۇلۇپلاش
        function lockSystem() {
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('lockBtn').style.display = 'none';
            document.getElementById('sessionTimer').style.display = 'none';
            document.getElementById('loginHint').textContent = 'شىفىرىنى كىرگۈزۈڭ';
            document.getElementById('loginError').textContent = '';
            currentPinInput = '';
            updatePinDisplay();
            stopSessionTimer();
        }
        
        // سېانىس ۋاقىت ھېسابى باشلاش
        function startSessionTimer() {
            const lockTime = parseInt(localStorage.getItem('autoLockTime') || '30');
            autoLockMinutes = lockTime;
            
            if (lockTime === 0) {
                document.getElementById('sessionTimer').style.display = 'none';
                return;
            }
            
            document.getElementById('sessionTimer').style.display = 'block';
            updateSessionDisplay();
            
            // ھەر سېكۇنت يېڭىلاش
            sessionTimeout = setInterval(() => {
                const elapsed = Math.floor((Date.now() - lastActivity) / 1000);
                const remaining = (autoLockMinutes * 60) - elapsed;
                
                if (remaining <= 0) {
                    lockSystem();
                    toast('ۋاقىت ئۆتۈپ كەتكەنلىكى ئۈچۈن قۇلۇپلاندى', '🔒');
                    return;
                }
                
                const timer = document.getElementById('sessionTimer');
                if (remaining <= 60) {
                    timer.classList.add('warning');
                } else {
                    timer.classList.remove('warning');
                }
                
                const mins = Math.floor(remaining / 60);
                const secs = remaining % 60;
                document.getElementById('sessionTime').textContent = 
                    `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
            }, 1000);
        }
        
        function updateSessionDisplay() {
            const mins = autoLockMinutes;
            document.getElementById('sessionTime').textContent = `${mins}:00`;
        }
        
        function stopSessionTimer() {
            if (sessionTimeout) clearInterval(sessionTimeout);
            if (sessionWarningTimeout) clearTimeout(sessionWarningTimeout);
        }
        
        // ئىشلەتكۈچى ھەرىكىتى
        function resetActivityTimer() {
            lastActivity = Date.now();
            const timer = document.getElementById('sessionTimer');
            if (timer) timer.classList.remove('warning');
        }
        
        // شىفىرە ئۆزگەرتىش
        async function changePin() {
            const current = document.getElementById('currentPin').value;
            const newPin = document.getElementById('newPin').value;
            const confirm = document.getElementById('confirmPin').value;
            
            if (!current || !newPin || !confirm) {
                toast('بارلىق رامكىلارنى تولدۇرۇڭ', '❌');
                return;
            }
            
            if (newPin.length !== 6 || !/^\d{6}$/.test(newPin)) {
                toast('شىفىرە 6 خانىلىق سان بولۇشى كېرەك', '❌');
                return;
            }
            
            if (newPin !== confirm) {
                toast('يېڭى شىفىرە ماس كەلمىدى', '❌');
                return;
            }
            
            const hashedCurrent = await hashPin(current);
            const savedPin = localStorage.getItem('systemPin');
            
            if (hashedCurrent !== savedPin) {
                toast('ھازىرقى شىفىرە خاتا', '❌');
                return;
            }
            
            const hashedNew = await hashPin(newPin);
            localStorage.setItem('systemPin', hashedNew);
            
            document.getElementById('currentPin').value = '';
            document.getElementById('newPin').value = '';
            document.getElementById('confirmPin').value = '';
            
            toast('شىفىرە مۇۋەپپەقىيەتلىك ئۆزگەرتىلدى!', '✅');
        }
        
        // ئاپتوماتىك قۇلۇپلاش ۋاقتى
        function saveAutoLockTime() {
            const time = document.getElementById('autoLockTime').value;
            localStorage.setItem('autoLockTime', time);
            autoLockMinutes = parseInt(time);
            stopSessionTimer();
            startSessionTimer();
            toast('ئاپتوماتىك قۇلۇپلاش ۋاقتى ساقلاندى', '✅');
        }
        
        // شىفىرىنى ئۇنتۇتتۇم
        function showForgotPin() {
            const answer = prompt('بىخەتەرلىك سوئالى:\nرېستوران نامىڭىزنى كىرگۈزۈڭ:');
            if (answer && answer.toLowerCase() === (data.settings?.name || 'رېستوران').toLowerCase()) {
                if (confirm('شىفىرىنى ئەسلىگە كەلتۈرەمسىز؟ يېڭى شىفىرە: 123456')) {
                    hashPin('123456').then(hash => {
                        localStorage.setItem('systemPin', hash);
                        toast('شىفىرە ئەسلىگە كەلتۈرۈلدى: 123456', '🔐');
                    });
                }
            } else if (answer) {
                toast('جاۋاب خاتا!', '❌');
            }
        }
        
        // ==================== IndexedDB سانلىق مەلۇمات ساقلاش ====================
        const DB_NAME = 'RestaurantDB';
        const DB_VERSION = 1;
        let db = null;
        
        // سانلىق مەلۇمات ئوبيېكتى
        let data = {
            menu: [],
            orders: [],
            tables: [],
            rooms: [],
            employees: [],
            expenses: [],
            cashboxes: { usd: 0, try: 0, cash: 0 },
            transactions: [],
            settings: {}
        };
        
        // IndexedDB ئېچىش
        function openDatabase() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open(DB_NAME, DB_VERSION);
                
                request.onerror = () => reject(request.error);
                request.onsuccess = () => {
                    db = request.result;
                    resolve(db);
                };
                
                request.onupgradeneeded = (event) => {
                    const database = event.target.result;
                    
                    // جەدۋەللەرنى قۇرۇش
                    const stores = ['menu', 'orders', 'tables', 'rooms', 'employees', 'expenses', 'transactions'];
                    stores.forEach(name => {
                        if (!database.objectStoreNames.contains(name)) {
                            database.createObjectStore(name, { keyPath: 'id' });
                        }
                    });
                    
                    // تەڭشەك ۋە ساندۇق ئۈچۈن ئايرىم store
                    if (!database.objectStoreNames.contains('settings')) {
                        database.createObjectStore('settings', { keyPath: 'key' });
                    }
                    if (!database.objectStoreNames.contains('cashboxes')) {
                        database.createObjectStore('cashboxes', { keyPath: 'key' });
                    }
                };
            });
        }
        
        // سانلىق مەلۇمات يۈكلەش
        async function loadAllData() {
            const stores = ['menu', 'orders', 'tables', 'rooms', 'employees', 'expenses', 'transactions'];
            
            for (const storeName of stores) {
                data[storeName] = await getAllFromStore(storeName);
            }
            
            // ساندۇق يۈكلەش
            const cashboxData = await getFromStore('cashboxes', 'main');
            if (cashboxData) {
                data.cashboxes = cashboxData.value;
            }
            
            // تەڭشەك يۈكلەش
            const settingsData = await getFromStore('settings', 'main');
            if (settingsData) {
                data.settings = settingsData.value;
            }
            
            // LocalStorage دىن كۆچۈرۈش (بىرىنچى قېتىم)
            await migrateFromLocalStorage();
        }
        
        // LocalStorage دىن IndexedDB غا كۆچۈرۈش
        async function migrateFromLocalStorage() {
            const stores = ['menu', 'orders', 'tables', 'rooms', 'employees', 'expenses', 'transactions'];
            let migrated = false;
            
            for (const storeName of stores) {
                const localData = localStorage.getItem(storeName);
                if (localData && data[storeName].length === 0) {
                    const items = JSON.parse(localData);
                    for (const item of items) {
                        await saveToStore(storeName, item);
                    }
                    data[storeName] = items;
                    migrated = true;
                }
            }
            
            // ساندۇق كۆچۈرۈش
            const localCashboxes = localStorage.getItem('cashboxes');
            if (localCashboxes && data.cashboxes.usd === 0 && data.cashboxes.try === 0) {
                data.cashboxes = JSON.parse(localCashboxes);
                await saveToStore('cashboxes', { key: 'main', value: data.cashboxes });
                migrated = true;
            }
            
            // تەڭشەك كۆچۈرۈش
            const localSettings = localStorage.getItem('settings');
            if (localSettings && Object.keys(data.settings).length === 0) {
                data.settings = JSON.parse(localSettings);
                await saveToStore('settings', { key: 'main', value: data.settings });
                migrated = true;
            }
            
            if (migrated) {
                console.log('LocalStorage دىن IndexedDB غا كۆچۈرۈلدى');
            }
        }
        
        // Store دىن ھەممىنى ئېلىش
        function getAllFromStore(storeName) {
            return new Promise((resolve, reject) => {
                const transaction = db.transaction(storeName, 'readonly');
                const store = transaction.objectStore(storeName);
                const request = store.getAll();
                request.onsuccess = () => resolve(request.result || []);
                request.onerror = () => reject(request.error);
            });
        }
        
        // Store دىن بىرنى ئېلىش
        function getFromStore(storeName, key) {
            return new Promise((resolve, reject) => {
                const transaction = db.transaction(storeName, 'readonly');
                const store = transaction.objectStore(storeName);
                const request = store.get(key);
                request.onsuccess = () => resolve(request.result);
                request.onerror = () => reject(request.error);
            });
        }
        
        // Store غا ساقلاش
        function saveToStore(storeName, item) {
            return new Promise((resolve, reject) => {
                const transaction = db.transaction(storeName, 'readwrite');
                const store = transaction.objectStore(storeName);
                const request = store.put(item);
                request.onsuccess = () => resolve(request.result);
                request.onerror = () => reject(request.error);
            });
        }
        
        // Store دىن ئۆچۈرۈش
        function deleteFromStore(storeName, id) {
            return new Promise((resolve, reject) => {
                const transaction = db.transaction(storeName, 'readwrite');
                const store = transaction.objectStore(storeName);
                const request = store.delete(id);
                request.onsuccess = () => resolve();
                request.onerror = () => reject(request.error);
            });
        }
        
        // ساقلاش فۇنكسىيەسى (يېڭى)
        async function save(key) {
            try {
                if (key === 'cashboxes') {
                    await saveToStore('cashboxes', { key: 'main', value: data.cashboxes });
                } else if (key === 'settings') {
                    await saveToStore('settings', { key: 'main', value: data.settings });
                }
                // باشقا ئوبيېكتلار ئۈچۈن ھەر بىر ئېلېمېنتنى ئايرىم ساقلاش
                updateStorage();
            } catch (error) {
                console.error('ساقلاش خاتالىقى:', error);
                toast('ساقلاش خاتالىقى!', '❌');
            }
        }
        
        // ئايرىم ئېلېمېنت ساقلاش
        async function saveItem(storeName, item) {
            try {
                await saveToStore(storeName, item);
                updateStorage();
            } catch (error) {
                console.error('ساقلاش خاتالىقى:', error);
            }
        }
        
        // ئايرىم ئېلېمېنت ئۆچۈرۈش
        async function deleteItem(storeName, id) {
            try {
                await deleteFromStore(storeName, id);
                data[storeName] = data[storeName].filter(x => x.id !== id);
                updateStorage();
            } catch (error) {
                console.error('ئۆچۈرۈش خاتالىقى:', error);
            }
        }
        
        let cart = [];
        let selectedLocation = null;
        let selectedPayment = 'usd';
        let exchangeRate = parseFloat(localStorage.getItem('exchangeRate') || '34.50');
        
        const categories = [
            { id: 'lagmen', name: 'لەغمەن', emoji: '🍜', color: 'from-amber-500 to-orange-600' },
            { id: 'quruma', name: 'قۇرۇما', emoji: '🥘', color: 'from-red-500 to-rose-600' },
            { id: 'qordaq', name: 'قورداق', emoji: '🍖', color: 'from-amber-600 to-red-600' },
            { id: 'kawap', name: 'كاۋاپ', emoji: '🍢', color: 'from-orange-500 to-red-600' },
            { id: 'shorpa', name: 'شورپا', emoji: '🍲', color: 'from-yellow-500 to-amber-600' },
            { id: 'seleng', name: 'سۇيۇق سەلەڭ', emoji: '🥣', color: 'from-teal-500 to-cyan-600' },
            { id: 'nan', name: 'نان', emoji: '🫓', color: 'from-amber-400 to-yellow-600' },
            { id: 'aqash', name: 'ئاق ئاش', emoji: '🍝', color: 'from-slate-400 to-gray-500' },
            { id: 'polo', name: 'پولو', emoji: '🍛', color: 'from-orange-400 to-amber-600' },
            { id: 'manta', name: 'مانتا', emoji: '🥟', color: 'from-rose-400 to-pink-600' },
            { id: 'paket', name: 'پاكەت', emoji: '🥡', color: 'from-emerald-500 to-green-600' },
            { id: 'drink', name: 'ئىچىملىك', emoji: '🧊', color: 'from-cyan-400 to-blue-500' },
            { id: 'tea', name: 'چاي', emoji: '🫖', color: 'from-green-500 to-emerald-600' }
        ];
        
        const expenseCategories = {
            vegetable: '🥬 سەي كۆكتات', meat: '🥩 گۆش', coal: '�ite كۆمۈر',
            utility: '💡 توك/سۇ', gas: '🔥 گاز', flour: '🌾 ئۇن',
            oil: '🛢️ ياغ', rice: '🍚 گۈرۈچ', salary: '💰 مائاش', other: '📦 باشقا'
        };
        
        const roleNames = { waiter: 'ئوفىتسىيانت', chef: 'ئاشپەز', cashier: 'كاسسىر', manager: 'مۇدىر' };
        const weekDays = ['يەكشەنبە', 'دۈشەنبە', 'سەيشەنبە', 'چارشەنبە', 'پەيشەنبە', 'جۈمە', 'شەنبە'];
        const months = ['يانۋار', 'فېۋرال', 'مارت', 'ئاپرېل', 'ماي', 'ئىيۇن', 'ئىيۇل', 'ئاۋغۇست', 'سېنتەبىر', 'ئۆكتەبىر', 'نويابىر', 'دېكابىر'];
        
        // ==================== باشلىنىش ====================
        document.addEventListener('DOMContentLoaded', async () => {
            console.log('سېستىما يۈكلىنىۋاتىدۇ...');
            
            // ھەرىكەت كۆزىتىش
            document.addEventListener('click', resetActivityTimer);
            document.addEventListener('keypress', resetActivityTimer);
            document.addEventListener('touchstart', resetActivityTimer);
            document.addEventListener('scroll', resetActivityTimer);
            
            // URL پارامېتىرىنى تەكشۈرۈش - خېرىدار ھالىتى (ئالدى تەكشۈرۈش)
            const urlParams = new URLSearchParams(window.location.search);
            const isCustomerMode = urlParams.has('customer') || urlParams.has('order') || urlParams.has('menu');
            
            if (isCustomerMode) {
                console.log('خېرىدار ھالىتى بايقالدى...');
                // خېرىدار ھالىتى - بىۋاسىتە خېرىدار بېتىنى ئېچىش
                await openCustomerModeDirect();
                return;
            }
            
            // باشقۇرغۇچى ھالىتى
            try {
                // IndexedDB ئېچىش
                await openDatabase();
                // سانلىق مەلۇمات يۈكلەش
                await loadAllData();
            } catch (error) {
                console.error('IndexedDB خاتالىقى:', error);
                // LocalStorage غا قايتىش
                loadFromLocalStorage();
            }
            
            // شىفىرە تەڭشەلگەنمۇ؟
            const savedPin = localStorage.getItem('systemPin');
            if (!savedPin) {
                // بىرىنچى قېتىم - شىفىرە تەڭشەش
                document.getElementById('loginHint').textContent = 'يېڭى 6 خانىلىق شىفىرە تەڭشەڭ';
            }
            
            // ئاپتوماتىك قۇلۇپلاش ۋاقتىنى يۈكلەش
            const savedLockTime = localStorage.getItem('autoLockTime');
            if (savedLockTime) {
                autoLockMinutes = parseInt(savedLockTime);
            }
            
            // يۈكلەش ئېكرانىنى يوشۇرۇش
            document.getElementById('loadingScreen').style.display = 'none';
            
            console.log('باشقۇرغۇچى ھالىتى تەييار');
        });
        
        // خېرىدار ھالىتىنى بىۋاسىتە ئېچىش (URL ئارقىلىق)
        async function openCustomerModeDirect() {
            console.log('خېرىدار ھالىتى ئېچىلىۋاتىدۇ...');
            
            // يۈكلەش ئېكرانىنى كۆرسىتىش
            const loadingScreen = document.getElementById('loadingScreen');
            if (loadingScreen) loadingScreen.style.display = 'flex';
            
            // كىرىش ئېكرانىنى يوشۇرۇش
            const loginScreen = document.getElementById('loginScreen');
            if (loginScreen) loginScreen.style.display = 'none';
            
            // IndexedDB ئېچىش
            try {
                await openDatabase();
                await loadAllData();
            } catch (error) {
                console.error('IndexedDB خاتالىقى:', error);
                loadFromLocalStorage();
            }
            
            // سۈكۈتتىكى سانلىق مەلۇماتلارنى قۇرۇش
            await initDefaultData();
            
            // يۈكلەش ئېكرانىنى يوشۇرۇش
            if (loadingScreen) loadingScreen.style.display = 'none';
            
            // باشقا ئېلېمېنتلارنى يوشۇرۇش
            const header = document.querySelector('.header');
            const bottomNav = document.querySelector('.bottom-nav');
            const lockBtn = document.getElementById('lockBtn');
            const sessionTimer = document.getElementById('sessionTimer');
            
            if (header) header.style.display = 'none';
            if (bottomNav) bottomNav.style.display = 'none';
            if (lockBtn) lockBtn.style.display = 'none';
            if (sessionTimer) sessionTimer.style.display = 'none';
            
            // خېرىدار بېتىنى كۆرسىتىش
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            const customerPage = document.getElementById('page-customer');
            if (customerPage) {
                customerPage.classList.add('active');
                customerPage.style.paddingBottom = '200px';
            }
            
            // خېرىدار بېتىنى يۈكلەش
            loadCustomerPage();
            
            // باشقۇرۇش كۇنۇپكىسىنى يوشۇرۇش (URL ئارقىلىق كىرگەندە)
            setTimeout(() => {
                const exitBtn = document.getElementById('exitCustomerBtn');
                if (exitBtn) {
                    exitBtn.style.display = 'none';
                }
            }, 100);
            
            console.log('خېرىدار ھالىتى ئېچىلدى ✅');
        }
        
        // LocalStorage دىن يۈكلەش (زاپاس پىلان)
        function loadFromLocalStorage() {
            data = {
                menu: JSON.parse(localStorage.getItem('menu') || '[]'),
                orders: JSON.parse(localStorage.getItem('orders') || '[]'),
                tables: JSON.parse(localStorage.getItem('tables') || '[]'),
                rooms: JSON.parse(localStorage.getItem('rooms') || '[]'),
                employees: JSON.parse(localStorage.getItem('employees') || '[]'),
                expenses: JSON.parse(localStorage.getItem('expenses') || '[]'),
                cashboxes: JSON.parse(localStorage.getItem('cashboxes') || '{"usd":0,"try":0,"cash":0}'),
                transactions: JSON.parse(localStorage.getItem('transactions') || '[]'),
                settings: JSON.parse(localStorage.getItem('settings') || '{}')
            };
        }
        
        function init() {
            initDefaultData();
            updateDateTime();
            setInterval(updateDateTime, 1000);
            updateStorage();
            loadDashboard();
            
            // فورما ھادىسىلىرى
            document.getElementById('menuForm').onsubmit = handleMenuSubmit;
            document.getElementById('tableForm').onsubmit = handleTableSubmit;
            document.getElementById('roomForm').onsubmit = handleRoomSubmit;
            document.getElementById('employeeForm').onsubmit = handleEmployeeSubmit;
            document.getElementById('expenseForm').onsubmit = handleExpenseSubmit;
            document.getElementById('cashboxForm').onsubmit = handleCashboxSubmit;
            document.getElementById('transferForm').onsubmit = handleTransferSubmit;
            document.getElementById('transferAmount').oninput = calcTransferResult;
            
            // تەڭشەكلەرنى يۈكلەش
            if (data.settings.name) document.getElementById('settingName').value = data.settings.name;
            if (data.settings.phone) document.getElementById('settingPhone').value = data.settings.phone;
            if (data.settings.address) document.getElementById('settingAddress').value = data.settings.address;
            document.getElementById('exchangeRate').value = exchangeRate;
            
            // ئاپتوماتىك قۇلۇپلاش ۋاقتى
            const savedLockTime = localStorage.getItem('autoLockTime');
            if (savedLockTime && document.getElementById('autoLockTime')) {
                document.getElementById('autoLockTime').value = savedLockTime;
            }
        }
        
        async function initDefaultData() {
            if (data.tables.length === 0) {
                for (let i = 1; i <= 10; i++) {
                    const table = { id: genId(), number: i, capacity: 4, status: 'free' };
                    data.tables.push(table);
                    await saveItem('tables', table);
                }
            }
            if (data.rooms.length === 0) {
                const rooms = [
                    { id: genId(), name: 'VIP-1', capacity: 8, price: 50, status: 'free' },
                    { id: genId(), name: 'VIP-2', capacity: 10, price: 80, status: 'free' }
                ];
                for (const room of rooms) {
                    data.rooms.push(room);
                    await saveItem('rooms', room);
                }
            }
            if (data.menu.length === 0) {
                const defaults = [
                    { name: 'سۇيۇق لەغمەن', category: 'lagmen', price: 25 },
                    { name: 'قۇرۇق لەغمەن', category: 'lagmen', price: 28 },
                    { name: 'گۆش قۇرۇما', category: 'quruma', price: 35 },
                    { name: 'قورداق', category: 'qordaq', price: 40 },
                    { name: 'تونۇر كاۋاپ', category: 'kawap', price: 8 },
                    { name: 'گۆش شورپا', category: 'shorpa', price: 25 },
                    { name: 'توقاچ نان', category: 'nan', price: 3 },
                    { name: 'پولو', category: 'polo', price: 35 },
                    { name: 'كولا', category: 'drink', price: 8 },
                    { name: 'چاي', category: 'tea', price: 5 }
                ];
                for (const m of defaults) {
                    const item = { id: genId(), ...m, available: true };
                    data.menu.push(item);
                    await saveItem('menu', item);
                }
            }
        }
        
        // ==================== ياردەمچى ====================
        function genId() { return Date.now().toString(36) + Math.random().toString(36).substr(2); }
        function save(key) { localStorage.setItem(key, JSON.stringify(data[key])); updateStorage(); }
        function formatCurrency(n, type = 'usd') { return type === 'try' ? '₺' + n.toFixed(2) : '$' + n.toFixed(2); }
        
        function updateDateTime() {
            const now = new Date();
            const day = weekDays[now.getDay()];
            const month = months[now.getMonth()];
            document.getElementById('currentTime').textContent = now.toLocaleTimeString('en-US', { hour12: false });
            document.getElementById('currentDate').textContent = `${now.getFullYear()}-يىل ${month} ${now.getDate()}-كۈنى، ${day}`;
        }
        
        async function updateStorage() {
            try {
                // IndexedDB بوشلۇقىنى ھېسابلاش
                if (navigator.storage && navigator.storage.estimate) {
                    const estimate = await navigator.storage.estimate();
                    const used = estimate.usage || 0;
                    const quota = estimate.quota || 100 * 1024 * 1024; // 100MB سۈكۈتتىكى
                    const mb = (used / 1024 / 1024).toFixed(2);
                    const percent = (used / quota * 100).toFixed(1);
                    document.getElementById('storageInfo').textContent = mb + ' MB / ' + (quota / 1024 / 1024).toFixed(0) + ' MB';
                    document.getElementById('storageBar').style.width = percent + '%';
                    
                    // ئاگاھلاندۇرۇش
                    if (percent > 80) {
                        document.getElementById('storageBar').style.background = 'linear-gradient(90deg,#ef4444,#f59e0b)';
                    } else {
                        document.getElementById('storageBar').style.background = 'linear-gradient(90deg,#6366f1,#8b5cf6)';
                    }
                } else {
                    // ئەسلى ئۇسۇل
                    const bytes = new Blob([JSON.stringify(data)]).size;
                    const mb = (bytes / 1024 / 1024).toFixed(2);
                    document.getElementById('storageInfo').textContent = mb + ' MB / 100 MB';
                    document.getElementById('storageBar').style.width = Math.min(bytes / 1024 / 1024, 100) + '%';
                }
            } catch (error) {
                const bytes = new Blob([JSON.stringify(data)]).size;
                const mb = (bytes / 1024 / 1024).toFixed(2);
                document.getElementById('storageInfo').textContent = mb + ' MB';
                document.getElementById('storageBar').style.width = Math.min(bytes / 1024 / 1024, 100) + '%';
            }
        }
        
        function toast(msg, icon = '✅') {
            const t = document.getElementById('toast');
            document.getElementById('toastIcon').textContent = icon;
            document.getElementById('toastMessage').textContent = msg;
            t.classList.add('show');
            setTimeout(() => t.classList.remove('show'), 3000);
        }
        
        function getCatEmoji(cat) { return categories.find(c => c.id === cat)?.emoji || '🍽️'; }
        function getCatName(cat) { return categories.find(c => c.id === cat)?.name || cat; }
        function getCatColor(cat) { return categories.find(c => c.id === cat)?.color || 'from-gray-500 to-gray-600'; }
        
        // ==================== يول باشلاش ====================
        function showPage(name) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById('page-' + name).classList.add('active');
            document.querySelectorAll('.nav-item-luxury').forEach(n => n.classList.remove('active'));
            document.querySelectorAll('.sidebar-item').forEach(n => n.classList.remove('active'));
            
            toggleSidebar(false);
            
            switch(name) {
                case 'dashboard': loadDashboard(); break;
                case 'pos': loadPOS(); break;
                case 'menu': loadMenu(); break;
                case 'orders': loadOrders(); break;
                case 'tables': loadTables(); break;
                case 'rooms': loadRooms(); break;
                case 'employees': loadEmployees(); break;
                case 'expenses': loadExpenses(); break;
                case 'cashbox': loadCashbox(); break;
                case 'reports': loadReports(); break;
                case 'settings': loadSettings(); break;
                case 'customer': loadCustomerPage(); break;
            }
        }
        
        function toggleSidebar(show) {
            const s = document.getElementById('sidebar');
            const o = document.getElementById('sidebarOverlay');
            if (show === undefined) show = !s.classList.contains('active');
            if (show) { s.classList.add('active'); o.classList.add('active'); }
            else { s.classList.remove('active'); o.classList.remove('active'); }
        }
        
        function openModal(id) { document.getElementById(id).classList.add('active'); }
        function closeModal(id) { document.getElementById(id).classList.remove('active'); }
        
        // ==================== باش بەت ====================
        function loadDashboard() {
            const today = new Date().toDateString();
            const thisMonth = new Date().getMonth();
            const thisYear = new Date().getFullYear();
            
            const todayOrders = data.orders.filter(o => new Date(o.createdAt).toDateString() === today && o.status === 'completed');
            const todayRev = todayOrders.reduce((s, o) => s + o.total, 0);
            const todayExp = data.expenses.filter(e => new Date(e.createdAt).toDateString() === today).reduce((s, e) => s + e.amount, 0);
            
            document.getElementById('todayRevenue').textContent = formatCurrency(todayRev);
            document.getElementById('todayExpense').textContent = formatCurrency(todayExp);
            document.getElementById('todayOrders').textContent = todayOrders.length;
            document.getElementById('todayProfit').textContent = formatCurrency(todayRev - todayExp);
            
            const monthOrders = data.orders.filter(o => {
                const d = new Date(o.createdAt);
                return d.getMonth() === thisMonth && d.getFullYear() === thisYear && o.status === 'completed';
            });
            const monthRev = monthOrders.reduce((s, o) => s + o.total, 0);
            const monthExp = data.expenses.filter(e => {
                const d = new Date(e.createdAt);
                return d.getMonth() === thisMonth && d.getFullYear() === thisYear;
            }).reduce((s, e) => s + e.amount, 0);
            
            document.getElementById('monthRevenue').textContent = formatCurrency(monthRev);
            document.getElementById('monthExpense').textContent = formatCurrency(monthExp);
            document.getElementById('monthProfit').textContent = formatCurrency(monthRev - monthExp);
            
            // ئاممىباب تائاملار
            loadPopularFoods();
            
            // يېقىنقى زاكازلار
            const recent = data.orders.slice(-5).reverse();
            document.getElementById('recentOrders').innerHTML = recent.length ? recent.map(o => `
                <div class="card p-3 mb-2">
                    <div class="flex items-center justify-between">
                        <div>
                            <span class="font-bold">#${o.id.slice(-6)}</span>
                            <span class="text-xs text-gray"> - ${o.location || ''}</span>
                        </div>
                        <span class="text-green font-bold">${formatCurrency(o.total)}</span>
                    </div>
                    <div class="text-xs text-gray mt-1">👤 ${o.customer || 'نامەلۇم'} | 🧑‍💼 ${o.employee || ''} | 💳 ${o.paymentName || ''}</div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">زاكاز يوق</p>';
        }
        
        function loadPopularFoods() {
            // تائام سېتىلىش سانىنى ھېسابلاش
            const foodCounts = {};
            data.orders.filter(o => o.status === 'completed').forEach(order => {
                order.items.forEach(item => {
                    if (!foodCounts[item.name]) {
                        foodCounts[item.name] = { name: item.name, count: 0, total: 0 };
                    }
                    foodCounts[item.name].count += item.qty;
                    foodCounts[item.name].total += item.price * item.qty;
                });
            });
            
            // ئەڭ كۆپ سېتىلغان 6 تائام
            const popular = Object.values(foodCounts)
                .sort((a, b) => b.count - a.count)
                .slice(0, 6);
            
            if (popular.length === 0) {
                document.getElementById('popularFoods').innerHTML = '<p class="text-center text-gray p-4" style="grid-column:span 3;">سانلىق مەلۇمات يوق</p>';
                return;
            }
            
            document.getElementById('popularFoods').innerHTML = popular.map((f, i) => {
                const menuItem = data.menu.find(m => m.name === f.name);
                const emoji = menuItem ? getCatEmoji(menuItem.category) : '🍽️';
                const medal = i === 0 ? '🥇' : i === 1 ? '🥈' : i === 2 ? '🥉' : '';
                return `
                    <div class="p-3 rounded-lg text-center" style="background:rgba(255,255,255,0.05);">
                        <div class="text-2xl mb-1">${medal}${emoji}</div>
                        <div class="text-xs font-bold mb-1">${f.name}</div>
                        <div class="text-xs text-green">${f.count} دانە</div>
                        <div class="text-xs text-gray">${formatCurrency(f.total)}</div>
                    </div>
                `;
            }).join('');
        }
        
        // ==================== POS ====================
        function loadPOS() {
            document.getElementById('posStep1').style.display = 'block';
            document.getElementById('posStep2').style.display = 'none';
            document.getElementById('posStep3').style.display = 'none';
            selectedLocation = null;
            
            // ئۈستەللەر
            document.getElementById('posTablesGrid').innerHTML = data.tables.map(t => `
                <div class="location-card ${t.status}" onclick="selectLocation('table', '${t.id}')">
                    <div style="font-size:24px;">${t.status === 'occupied' ? '🔴' : '🪑'}</div>
                    <div class="font-bold">${t.number}</div>
                    <div class="text-xs text-gray">${t.capacity} ئورۇن</div>
                </div>
            `).join('');
            
            // خانىلار
            document.getElementById('posRoomsGrid').innerHTML = data.rooms.map(r => `
                <div class="location-card ${r.status === 'occupied' ? 'occupied' : 'free'}" onclick="selectLocation('room', '${r.id}')" style="padding:16px;">
                    <div class="flex items-center gap-3">
                        <span style="font-size:28px;">${r.status === 'occupied' ? '🔒' : '🚪'}</span>
                        <div class="text-right">
                            <div class="font-bold">${r.name}</div>
                            <div class="text-xs text-gray">${r.capacity} ئورۇن</div>
                        </div>
                    </div>
                </div>
            `).join('');
        }
        
        function selectLocation(type, id) {
            if (type === 'takeaway') {
                selectedLocation = { type: 'takeaway', name: 'ئېلىپ كېتىش', icon: '🛍️' };
            } else if (type === 'table') {
                const t = data.tables.find(x => x.id === id);
                if (t.status === 'occupied') { toast('بۇ ئۈستەل ئىگىلەنگەن!', '❌'); return; }
                selectedLocation = { type: 'table', id, name: 'ئۈستەل ' + t.number, icon: '🪑' };
            } else if (type === 'room') {
                const r = data.rooms.find(x => x.id === id);
                if (r.status === 'occupied') { toast('بۇ خانا ئىگىلەنگەن!', '❌'); return; }
                selectedLocation = { type: 'room', id, name: r.name, icon: '🚪' };
            }
            goToStep2();
        }
        
        function goToStep2() {
            document.getElementById('posStep1').style.display = 'none';
            document.getElementById('posStep2').style.display = 'block';
            document.getElementById('selectedLocationIcon').textContent = selectedLocation.icon;
            document.getElementById('selectedLocationName').textContent = selectedLocation.name;
            
            // خىزمەتچى تىزىملىكى
            document.getElementById('posEmployee').innerHTML = '<option value="">تاللاڭ...</option>' +
                data.employees.map(e => `<option value="${e.id}">${e.name} - ${roleNames[e.role]}</option>`).join('');
            
            // تۈر سۈزگۈچ
            document.getElementById('posCategoryFilter').innerHTML = 
                '<button class="filter-btn active" onclick="filterPOS(\'all\')">✨ ھەممىسى</button>' +
                categories.map(c => `<button class="filter-btn" onclick="filterPOS('${c.id}')">${c.emoji} ${c.name}</button>`).join('');
            
            loadPOSMenu('all');
        }
        
        function backToStep1() {
            document.getElementById('posStep1').style.display = 'block';
            document.getElementById('posStep2').style.display = 'none';
            document.getElementById('posStep3').style.display = 'none';
            selectedLocation = null;
        }
        
        function filterPOS(cat) {
            document.querySelectorAll('#posCategoryFilter .filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            loadPOSMenu(cat);
        }
        
        function loadPOSMenu(cat) {
            let items = data.menu.filter(m => m.available);
            if (cat !== 'all') items = items.filter(m => m.category === cat);
            
            const catColors = {
                lagmen: 'rgba(245,158,11,0.4),rgba(234,88,12,0.3)',
                quruma: 'rgba(239,68,68,0.4),rgba(185,28,28,0.3)',
                qordaq: 'rgba(249,115,22,0.4),rgba(194,65,12,0.3)',
                kawap: 'rgba(239,68,68,0.4),rgba(220,38,38,0.3)',
                shorpa: 'rgba(234,179,8,0.4),rgba(202,138,4,0.3)',
                seleng: 'rgba(20,184,166,0.4),rgba(13,148,136,0.3)',
                nan: 'rgba(245,158,11,0.4),rgba(217,119,6,0.3)',
                aqash: 'rgba(148,163,184,0.4),rgba(100,116,139,0.3)',
                polo: 'rgba(249,115,22,0.4),rgba(234,88,12,0.3)',
                manta: 'rgba(244,114,182,0.4),rgba(219,39,119,0.3)',
                paket: 'rgba(16,185,129,0.4),rgba(5,150,105,0.3)',
                drink: 'rgba(6,182,212,0.4),rgba(8,145,178,0.3)',
                tea: 'rgba(34,197,94,0.4),rgba(22,163,74,0.3)'
            };
            
            document.getElementById('posMenuGrid').innerHTML = items.map(m => `
                <div class="food-card-luxury" onclick="addToCart('${m.id}')" style="background:linear-gradient(145deg, ${catColors[m.category] || 'rgba(99,102,241,0.4),rgba(139,92,246,0.3)'});">
                    <span class="emoji">${getCatEmoji(m.category)}</span>
                    <div class="name" style="font-size:12px;font-weight:bold;margin-bottom:6px;color:white;text-shadow:0 2px 4px rgba(0,0,0,0.3);">${m.name}</div>
                    <div style="background:rgba(0,0,0,0.4);padding:4px 12px;border-radius:12px;display:inline-block;">
                        <span style="color:#10b981;font-weight:bold;font-size:13px;">${formatCurrency(m.price)}</span>
                    </div>
                </div>
            `).join('');
        }
        
        function addToCart(id) {
            const item = data.menu.find(m => m.id === id);
            if (!item) return;
            const existing = cart.find(c => c.id === id);
            if (existing) existing.qty++;
            else cart.push({ ...item, qty: 1 });
            updateCart();
            if (cart.length > 0) document.getElementById('posStep3').style.display = 'block';
        }
        
        function updateCart() {
            const total = cart.reduce((s, c) => s + c.price * c.qty, 0);
            document.getElementById('cartCount').textContent = cart.reduce((s, c) => s + c.qty, 0);
            document.getElementById('cartTotal').textContent = formatCurrency(total);
            document.getElementById('cartTotalTRY').textContent = formatCurrency(total * exchangeRate, 'try');
            
            document.getElementById('cartItems').innerHTML = cart.length ? cart.map(c => `
                <div class="cart-item">
                    <div class="flex-1">
                        <div class="font-bold text-sm">${getCatEmoji(c.category)} ${c.name}</div>
                        <div class="text-xs text-green">${formatCurrency(c.price)} × ${c.qty} = ${formatCurrency(c.price * c.qty)}</div>
                    </div>
                    <div class="flex items-center gap-2">
                        <button class="qty-btn minus" onclick="updateQty('${c.id}', -1)">−</button>
                        <span class="font-bold">${c.qty}</span>
                        <button class="qty-btn plus" onclick="updateQty('${c.id}', 1)">+</button>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-2">سېۋەت بوش</p>';
            
            if (cart.length === 0) document.getElementById('posStep3').style.display = 'none';
        }
        
        function updateQty(id, delta) {
            const item = cart.find(c => c.id === id);
            if (!item) return;
            item.qty += delta;
            if (item.qty <= 0) cart = cart.filter(c => c.id !== id);
            updateCart();
        }
        
        function clearCart() {
            cart = [];
            updateCart();
        }
        
        function selectPayment(type) {
            selectedPayment = type;
            document.querySelectorAll('.pay-btn').forEach(b => {
                b.style.background = 'rgba(255,255,255,0.05)';
                b.style.borderColor = 'transparent';
            });
            const colors = { usd: '#10b981', try: '#f59e0b', cash: '#8b5cf6' };
            event.target.style.background = `rgba(${type === 'usd' ? '16,185,129' : type === 'try' ? '245,158,11' : '139,92,246'},0.3)`;
            event.target.style.borderColor = colors[type];
            document.getElementById('liraConvert').style.display = type === 'try' ? 'block' : 'none';
        }
        
        function completeSale() {
            if (!cart.length) { toast('سېۋەت بوش!', '❌'); return; }
            if (!selectedLocation) { toast('ئورۇن تاللاڭ!', '❌'); return; }
            
            const empId = document.getElementById('posEmployee').value;
            if (!empId) { toast('خىزمەتچى تاللاڭ!', '❌'); return; }
            
            const emp = data.employees.find(e => e.id === empId);
            const total = cart.reduce((s, c) => s + c.price * c.qty, 0);
            const payNames = { usd: 'دوللار', try: 'لىرا', cash: 'شامكاش' };
            
            // ئورۇن ئىگىلەش
            if (selectedLocation.type === 'table') {
                const t = data.tables.find(x => x.id === selectedLocation.id);
                if (t) { t.status = 'occupied'; save('tables'); }
            } else if (selectedLocation.type === 'room') {
                const r = data.rooms.find(x => x.id === selectedLocation.id);
                if (r) { r.status = 'occupied'; save('rooms'); }
            }
            
            const order = {
                id: genId(),
                items: cart.map(c => ({ name: c.name, qty: c.qty, price: c.price })),
                total,
                location: selectedLocation.name,
                locationType: selectedLocation.type,
                locationId: selectedLocation.id,
                customer: document.getElementById('posCustomer').value || 'نامەلۇم',
                employee: emp?.name || '',
                employeeId: empId,
                paymentMethod: selectedPayment,
                paymentName: payNames[selectedPayment],
                status: 'completed',
                createdAt: new Date().toISOString()
            };
            
            data.orders.push(order);
            save('orders');
            
            // ساندۇققا قوشۇش
            const amount = selectedPayment === 'try' ? total * exchangeRate : total;
            data.cashboxes[selectedPayment] += amount;
            data.transactions.push({
                id: genId(),
                type: 'in',
                cashbox: selectedPayment,
                amount,
                amountUSD: total,
                note: `زاكاز #${order.id.slice(-6)} - ${selectedLocation.name}`,
                person: emp?.name || '',
                createdAt: new Date().toISOString()
            });
            save('cashboxes');
            save('transactions');
            
            showReceipt(order);
            cart = [];
            updateCart();
            selectedLocation = null;
            loadPOS();
            toast('زاكاز تاماملاندى! ✅');
        }
        
        function showReceipt(order) {
            const s = data.settings;
            document.getElementById('receiptContent').innerHTML = `
                <h2 class="text-center">${s.name || 'رېستوران'}</h2>
                <p class="text-center text-sm" style="color:#666;">${s.address || ''}</p>
                <p class="text-center text-sm" style="color:#666;">${s.phone || ''}</p>
                <hr>
                <p><strong>نومۇر:</strong> #${order.id.slice(-6)}</p>
                <p><strong>ۋاقتى:</strong> ${new Date(order.createdAt).toLocaleString()}</p>
                <p><strong>ئورنى:</strong> ${order.location}</p>
                <p><strong>خېرىدار:</strong> ${order.customer}</p>
                <p><strong>ساتقۇچى:</strong> ${order.employee}</p>
                <hr>
                <table>
                    <tr><th>تائام</th><th>سانى</th><th>باھاسى</th></tr>
                    ${order.items.map(i => `<tr><td>${i.name}</td><td>${i.qty}</td><td>${formatCurrency(i.price * i.qty)}</td></tr>`).join('')}
                </table>
                <hr>
                <p style="font-size:18px;"><strong>جەمئىي: ${formatCurrency(order.total)}</strong></p>
                <p><strong>تۆلەش:</strong> ${order.paymentName}</p>
                <hr>
                <p class="text-center" style="color:#666;">رەھمەت! يەنە كېلىڭ!</p>
            `;
            openModal('receiptModal');
        }
        
        function printReceipt() {
            if (!cart.length) { toast('سېۋەت بوش!', '❌'); return; }
            const order = {
                id: 'TEMP' + Date.now(),
                items: cart.map(c => ({ name: c.name, qty: c.qty, price: c.price })),
                total: cart.reduce((s, c) => s + c.price * c.qty, 0),
                location: selectedLocation?.name || '',
                customer: document.getElementById('posCustomer')?.value || '',
                employee: '',
                createdAt: new Date().toISOString()
            };
            showReceipt(order);
        }
        
        function printReceiptContent() { window.print(); }
        
        // ==================== تائام ====================
        function loadMenu(cat = 'all') {
            document.getElementById('menuCategoryFilter').innerHTML = 
                '<button class="filter-btn active" onclick="filterMenu(\'all\')">✨ ھەممىسى</button>' +
                categories.map(c => `<button class="filter-btn" onclick="filterMenu('${c.id}')">${c.emoji} ${c.name}</button>`).join('');
            
            document.getElementById('menuCategory').innerHTML = categories.map(c => 
                `<option value="${c.id}">${c.emoji} ${c.name}</option>`
            ).join('');
            
            let items = data.menu;
            if (cat !== 'all') items = items.filter(m => m.category === cat);
            
            document.getElementById('menuList').innerHTML = items.length ? items.map(m => `
                <div class="card mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">${getCatEmoji(m.category)}</span>
                        <div class="flex-1">
                            <h4 class="font-bold">${m.name}</h4>
                            <p class="text-xs text-gray">${getCatName(m.category)}</p>
                            <p class="text-green font-bold">${formatCurrency(m.price)}</p>
                        </div>
                        <span class="text-xs ${m.available ? 'text-green' : 'text-red'}">${m.available ? '✅ بار' : '❌ يوق'}</span>
                    </div>
                    <div class="grid grid-2 gap-2 mt-3" style="border-top:1px solid rgba(255,255,255,0.1);padding-top:12px;">
                        <button onclick="editMenu('${m.id}')" class="btn text-sm" style="background:rgba(99,102,241,0.2);color:#6366f1;">✏️ تەھرىر</button>
                        <button onclick="deleteMenu('${m.id}')" class="btn text-sm btn-danger">🗑️ ئۆچۈر</button>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">تائام يوق</p>';
        }
        
        let currentMenuCategory = 'all';
        
        function filterMenu(cat) {
            document.querySelectorAll('#menuCategoryFilter .filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            currentMenuCategory = cat;
            loadMenu(cat);
        }
        
        function searchMenu() {
            const query = document.getElementById('menuSearch').value.toLowerCase();
            
            let items = data.menu;
            
            // تۈر سۈزگۈچى
            if (currentMenuCategory !== 'all') {
                items = items.filter(m => m.category === currentMenuCategory);
            }
            
            // ئىزدەش
            if (query) {
                items = items.filter(m => 
                    m.name.toLowerCase().includes(query) ||
                    getCatName(m.category).toLowerCase().includes(query)
                );
            }
            
            renderMenu(items);
        }
        
        function renderMenu(items) {
            // ستاتىستىكا
            document.getElementById('menuCount').textContent = items.length;
            document.getElementById('menuAvailableCount').textContent = items.filter(m => m.available).length;
            document.getElementById('menuUnavailableCount').textContent = items.filter(m => !m.available).length;
            
            document.getElementById('menuList').innerHTML = items.length ? items.map(m => `
                <div class="card mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">${getCatEmoji(m.category)}</span>
                        <div class="flex-1">
                            <h4 class="font-bold">${m.name}</h4>
                            <p class="text-xs text-gray">${getCatName(m.category)}</p>
                            <p class="text-green font-bold">${formatCurrency(m.price)}</p>
                        </div>
                        <span class="text-xs ${m.available ? 'text-green' : 'text-red'}">${m.available ? '✅ بار' : '❌ يوق'}</span>
                    </div>
                    <div class="grid grid-3 gap-2 mt-3" style="border-top:1px solid rgba(255,255,255,0.1);padding-top:12px;">
                        <button onclick="toggleMenuAvailable('${m.id}')" class="btn text-xs ${m.available ? 'btn-warning' : 'btn-success'}">${m.available ? '❌ يوق' : '✅ بار'}</button>
                        <button onclick="editMenu('${m.id}')" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;">✏️ تەھرىر</button>
                        <button onclick="deleteMenu('${m.id}')" class="btn text-xs btn-danger">🗑️ ئۆچۈر</button>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">تائام يوق</p>';
        }
        
        async function toggleMenuAvailable(id) {
            const m = data.menu.find(x => x.id === id);
            if (m) {
                m.available = !m.available;
                await saveItem('menu', m);
                loadMenu(currentMenuCategory);
                toast(m.available ? 'تائام بار قىلىندى!' : 'تائام يوق قىلىندى!', m.available ? '✅' : '❌');
            }
        }
        
        function openMenuModal() { document.getElementById('menuId').value = ''; document.getElementById('menuForm').reset(); openModal('menuModal'); }
        function editMenu(id) {
            const m = data.menu.find(x => x.id === id);
            if (!m) return;
            document.getElementById('menuId').value = m.id;
            document.getElementById('menuName').value = m.name;
            document.getElementById('menuCategory').value = m.category;
            document.getElementById('menuPrice').value = m.price;
            document.getElementById('menuAvailable').checked = m.available;
            openModal('menuModal');
        }
        function deleteMenu(id) { if (confirm('ئۆچۈرەمسىز؟')) { data.menu = data.menu.filter(m => m.id !== id); save('menu'); loadMenu(); toast('ئۆچۈرۈلدى!'); } }
        function handleMenuSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('menuId').value;
            const item = {
                id: id || genId(),
                name: document.getElementById('menuName').value,
                category: document.getElementById('menuCategory').value,
                price: parseFloat(document.getElementById('menuPrice').value),
                available: document.getElementById('menuAvailable').checked
            };
            if (id) { const i = data.menu.findIndex(m => m.id === id); data.menu[i] = item; }
            else data.menu.push(item);
            save('menu'); closeModal('menuModal'); loadMenu(); toast('ساقلاندى!');
        }
        
        // ==================== زاكازلار ====================
        function loadOrders(filter = 'all', customerOnly = false) {
            let orders = [...data.orders].reverse();
            
            // خېرىدار زاكازلىرى سۈزگۈچى
            if (customerOnly) {
                orders = orders.filter(o => o.source === 'customer');
            }
            
            if (filter !== 'all') orders = orders.filter(o => o.status === filter);
            
            // يېڭى زاكاز بەلگىسىنى يېڭىلاش
            const pendingCustomer = data.orders.filter(o => o.source === 'customer' && o.status === 'pending');
            const badge = document.getElementById('newOrderBadge');
            if (badge) {
                if (pendingCustomer.length > 0) {
                    badge.style.display = 'block';
                    badge.textContent = '🔔 ' + pendingCustomer.length + ' يېڭى زاكاز!';
                } else {
                    badge.style.display = 'none';
                }
            }
            
            document.getElementById('ordersList').innerHTML = orders.length ? orders.map(o => `
                <div class="card mb-3" style="${o.source === 'customer' ? 'border:2px solid rgba(16,185,129,0.5);' : ''}">
                    <div class="flex items-center justify-between mb-2">
                        <div class="flex items-center gap-2">
                            <span class="font-bold">#${o.id.slice(-6)}</span>
                            ${o.source === 'customer' ? '<span style="background:rgba(16,185,129,0.2);color:#10b981;padding:2px 8px;border-radius:10px;font-size:10px;">📱 خېرىدار</span>' : ''}
                        </div>
                        <span class="text-xs ${o.status === 'completed' ? 'text-green' : o.status === 'preparing' ? 'text-blue' : 'text-yellow'}">
                            ${o.status === 'completed' ? '✅ تامام' : o.status === 'preparing' ? '🔥 تەييارلاۋاتقان' : '⏳ ساقلاۋاتقان'}
                        </span>
                    </div>
                    ${o.note ? `<div class="text-xs text-orange mb-2" style="background:rgba(249,115,22,0.1);padding:4px 8px;border-radius:6px;">📝 ${o.note}</div>` : ''}
                    <div class="text-xs text-gray mb-2">
                        📍 ${o.location} | 👤 ${o.customer} | 🧑‍💼 ${o.employee} | 💳 ${o.paymentName || ''}
                    </div>
                    <div class="text-xs mb-2">${o.items.map(i => `<span style="background:rgba(99,102,241,0.2);padding:2px 8px;border-radius:10px;margin:2px;display:inline-block;">${i.name} ×${i.qty}</span>`).join('')}</div>
                    <div class="flex items-center justify-between">
                        <span class="text-lg font-bold text-green">${formatCurrency(o.total)}</span>
                        <div class="flex gap-2">
                            ${o.status === 'pending' ? `<button onclick="updateOrder('${o.id}','preparing')" class="btn text-xs" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🔥</button>` : ''}
                            ${o.status === 'preparing' ? `<button onclick="updateOrder('${o.id}','completed')" class="btn text-xs" style="background:rgba(16,185,129,0.2);color:#10b981;">✅</button>` : ''}
                            <button onclick="showReceipt(data.orders.find(x=>x.id==='${o.id}'))" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;">🖨️</button>
                        </div>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">زاكاز يوق</p>';
        }
        
        let currentOrderFilter = 'all';
        let currentOrderCustomerOnly = false;
        
        function filterOrders(status) {
            document.querySelectorAll('#page-orders .filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            currentOrderFilter = status === 'customer' ? 'all' : status;
            currentOrderCustomerOnly = status === 'customer';
            loadOrders(currentOrderFilter, currentOrderCustomerOnly);
        }
        
        function searchOrders() {
            const query = document.getElementById('orderSearch').value.toLowerCase();
            const dateFilter = document.getElementById('orderDateFilter').value;
            
            let orders = [...data.orders].reverse();
            
            // ھالەت سۈزگۈچى
            if (currentOrderFilter !== 'all') {
                orders = orders.filter(o => o.status === currentOrderFilter);
            }
            
            // خېرىدار زاكازلىرى
            if (currentOrderCustomerOnly) {
                orders = orders.filter(o => o.source === 'customer');
            }
            
            // ئىزدەش
            if (query) {
                orders = orders.filter(o => 
                    o.id.toLowerCase().includes(query) ||
                    (o.customer && o.customer.toLowerCase().includes(query)) ||
                    (o.employee && o.employee.toLowerCase().includes(query)) ||
                    (o.location && o.location.toLowerCase().includes(query)) ||
                    o.items.some(i => i.name.toLowerCase().includes(query))
                );
            }
            
            // چېسلا سۈزگۈچى
            if (dateFilter) {
                orders = orders.filter(o => o.createdAt.split('T')[0] === dateFilter);
            }
            
            renderOrders(orders);
        }
        
        function filterOrdersByDate() {
            searchOrders();
        }
        
        function clearOrderFilters() {
            document.getElementById('orderSearch').value = '';
            document.getElementById('orderDateFilter').value = '';
            currentOrderFilter = 'all';
            currentOrderCustomerOnly = false;
            document.querySelectorAll('#page-orders .filter-btn').forEach(b => b.classList.remove('active'));
            document.querySelector('#page-orders .filter-btn').classList.add('active');
            loadOrders('all', false);
        }
        
        function renderOrders(orders) {
            document.getElementById('ordersCount').textContent = orders.length;
            
            document.getElementById('ordersList').innerHTML = orders.length ? orders.map(o => `
                <div class="card mb-3" style="${o.source === 'customer' ? 'border:2px solid rgba(16,185,129,0.5);' : ''}">
                    <div class="flex items-center justify-between mb-2">
                        <div class="flex items-center gap-2">
                            <span class="font-bold">#${o.id.slice(-6)}</span>
                            ${o.source === 'customer' ? '<span style="background:rgba(16,185,129,0.2);color:#10b981;padding:2px 8px;border-radius:10px;font-size:10px;">📱 خېرىدار</span>' : ''}
                        </div>
                        <span class="text-xs ${o.status === 'completed' ? 'text-green' : o.status === 'preparing' ? 'text-blue' : 'text-yellow'}">
                            ${o.status === 'completed' ? '✅ تامام' : o.status === 'preparing' ? '🔥 تەييارلاۋاتقان' : '⏳ ساقلاۋاتقان'}
                        </span>
                    </div>
                    ${o.note ? `<div class="text-xs text-orange mb-2" style="background:rgba(249,115,22,0.1);padding:4px 8px;border-radius:6px;">📝 ${o.note}</div>` : ''}
                    <div class="text-xs text-gray mb-2">
                        📍 ${o.location} | 👤 ${o.customer || 'نامەلۇم'} | 🧑‍💼 ${o.employee || ''} | 💳 ${o.paymentName || ''}
                    </div>
                    <div class="text-xs text-gray mb-2">🕐 ${new Date(o.createdAt).toLocaleString()}</div>
                    <div class="text-xs mb-2">${o.items.map(i => `<span style="background:rgba(99,102,241,0.2);padding:2px 8px;border-radius:10px;margin:2px;display:inline-block;">${i.name} ×${i.qty}</span>`).join('')}</div>
                    <div class="flex items-center justify-between">
                        <span class="text-lg font-bold text-green">${formatCurrency(o.total)}</span>
                        <div class="flex gap-2">
                            ${o.status === 'pending' ? `<button onclick="updateOrder('${o.id}','preparing')" class="btn text-xs" style="background:rgba(59,130,246,0.2);color:#3b82f6;">🔥</button>` : ''}
                            ${o.status === 'preparing' ? `<button onclick="updateOrder('${o.id}','completed')" class="btn text-xs" style="background:rgba(16,185,129,0.2);color:#10b981;">✅</button>` : ''}
                            <button onclick="showReceipt(data.orders.find(x=>x.id==='${o.id}'))" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;">🖨️</button>
                            <button onclick="deleteOrder('${o.id}')" class="btn text-xs btn-danger">🗑️</button>
                        </div>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">زاكاز يوق</p>';
        }
        
        async function deleteOrder(id) {
            if (!confirm('بۇ زاكازنى ئۆچۈرەمسىز؟')) return;
            
            const order = data.orders.find(o => o.id === id);
            if (order && order.status === 'completed') {
                // ساندۇقتىن پۇل چىقىرىش
                const payment = order.paymentMethod || 'usd';
                const amount = payment === 'try' ? order.total * exchangeRate : order.total;
                data.cashboxes[payment] -= amount;
                
                data.transactions.push({
                    id: genId(),
                    type: 'out',
                    cashbox: payment,
                    amount: amount,
                    note: `زاكاز ئۆچۈرۈلدى #${id.slice(-6)}`,
                    person: 'سېستىما',
                    createdAt: new Date().toISOString()
                });
                
                await save('cashboxes');
                await save('transactions');
            }
            
            await deleteItem('orders', id);
            loadOrders(currentOrderFilter, currentOrderCustomerOnly);
            loadDashboard();
            toast('زاكاز ئۆچۈرۈلدى!', '🗑️');
        }
        
        function updateOrder(id, status) {
            const o = data.orders.find(x => x.id === id);
            if (!o) return;
            o.status = status;
            if (status === 'completed' && o.locationId) {
                if (o.locationType === 'table') {
                    const t = data.tables.find(x => x.id === o.locationId);
                    if (t) { t.status = 'free'; save('tables'); }
                } else if (o.locationType === 'room') {
                    const r = data.rooms.find(x => x.id === o.locationId);
                    if (r) { r.status = 'free'; save('rooms'); }
                }
            }
            save('orders'); loadOrders(); toast('يېڭىلاندى!');
        }
        
        // ==================== ئۈستەللەر ====================
        function loadTables() {
            document.getElementById('tablesList').innerHTML = data.tables.map(t => `
                <div class="location-card ${t.status}" onclick="toggleTable('${t.id}')">
                    <span style="font-size:24px;">${t.status === 'occupied' ? '🔴' : '🪑'}</span>
                    <div class="font-bold">${t.number}</div>
                    <div class="text-xs text-gray">${t.capacity} ئورۇن</div>
                    <div class="grid grid-2 gap-1 mt-2">
                        <button onclick="event.stopPropagation();editTable('${t.id}')" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;padding:4px;">✏️</button>
                        <button onclick="event.stopPropagation();deleteTable('${t.id}')" class="btn text-xs btn-danger" style="padding:4px;">🗑️</button>
                    </div>
                </div>
            `).join('');
        }
        function toggleTable(id) { const t = data.tables.find(x => x.id === id); if (t) { t.status = t.status === 'free' ? 'occupied' : 'free'; save('tables'); loadTables(); } }
        function openTableModal() { document.getElementById('tableId').value = ''; document.getElementById('tableForm').reset(); openModal('tableModal'); }
        function editTable(id) {
            const t = data.tables.find(x => x.id === id);
            document.getElementById('tableId').value = t.id;
            document.getElementById('tableNumber').value = t.number;
            document.getElementById('tableCapacity').value = t.capacity;
            openModal('tableModal');
        }
        function deleteTable(id) { if (confirm('ئۆچۈرەمسىز؟')) { data.tables = data.tables.filter(t => t.id !== id); save('tables'); loadTables(); toast('ئۆچۈرۈلدى!'); } }
        function handleTableSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('tableId').value;
            const item = { id: id || genId(), number: parseInt(document.getElementById('tableNumber').value), capacity: parseInt(document.getElementById('tableCapacity').value), status: 'free' };
            if (id) { const i = data.tables.findIndex(t => t.id === id); data.tables[i] = { ...data.tables[i], ...item }; }
            else data.tables.push(item);
            save('tables'); closeModal('tableModal'); loadTables(); loadPOS(); toast('ساقلاندى!');
        }
        
        // ==================== ئايرىمخانىلار ====================
        function loadRooms() {
            document.getElementById('roomsList').innerHTML = data.rooms.map(r => `
                <div class="card mb-3" style="border:2px solid ${r.status === 'occupied' ? 'rgba(239,68,68,0.5)' : 'rgba(16,185,129,0.5)'};">
                    <div class="flex items-center gap-3 mb-3">
                        <span style="font-size:32px;">${r.status === 'occupied' ? '🔒' : '🚪'}</span>
                        <div class="flex-1">
                            <h4 class="font-bold">${r.name}</h4>
                            <p class="text-xs text-gray">${r.capacity} ئورۇن</p>
                            ${r.price > 0 ? `<p class="text-xs text-yellow">سائەتلىك: ${formatCurrency(r.price)}</p>` : ''}
                        </div>
                    </div>
                    <div class="grid grid-3 gap-2">
                        <button onclick="toggleRoom('${r.id}')" class="btn text-xs ${r.status === 'occupied' ? 'btn-success' : 'btn-warning'}">${r.status === 'occupied' ? '🔓 بوشات' : '🔒 ئىگىلە'}</button>
                        <button onclick="editRoom('${r.id}')" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;">✏️</button>
                        <button onclick="deleteRoom('${r.id}')" class="btn text-xs btn-danger">🗑️</button>
                    </div>
                </div>
            `).join('');
        }
        function toggleRoom(id) { const r = data.rooms.find(x => x.id === id); if (r) { r.status = r.status === 'free' ? 'occupied' : 'free'; save('rooms'); loadRooms(); } }
        function openRoomModal() { document.getElementById('roomId').value = ''; document.getElementById('roomForm').reset(); openModal('roomModal'); }
        function editRoom(id) {
            const r = data.rooms.find(x => x.id === id);
            document.getElementById('roomId').value = r.id;
            document.getElementById('roomName').value = r.name;
            document.getElementById('roomCapacity').value = r.capacity;
            document.getElementById('roomPrice').value = r.price || 0;
            openModal('roomModal');
        }
        function deleteRoom(id) { if (confirm('ئۆچۈرەمسىز؟')) { data.rooms = data.rooms.filter(r => r.id !== id); save('rooms'); loadRooms(); toast('ئۆچۈرۈلدى!'); } }
        function handleRoomSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('roomId').value;
            const item = { id: id || genId(), name: document.getElementById('roomName').value, capacity: parseInt(document.getElementById('roomCapacity').value), price: parseFloat(document.getElementById('roomPrice').value) || 0, status: 'free' };
            if (id) { const i = data.rooms.findIndex(r => r.id === id); data.rooms[i] = { ...data.rooms[i], ...item }; }
            else data.rooms.push(item);
            save('rooms'); closeModal('roomModal'); loadRooms(); loadPOS(); toast('ساقلاندى!');
        }
        
        // ==================== خىزمەتچىلەر ====================
        function loadEmployees() {
            document.getElementById('employeesList').innerHTML = data.employees.length ? data.employees.map(e => `
                <div class="card mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:32px;">👤</span>
                        <div class="flex-1">
                            <h4 class="font-bold">${e.name}</h4>
                            <p class="text-xs text-gray">${roleNames[e.role] || e.role}</p>
                            <p class="text-xs text-gray">${e.phone || ''}</p>
                        </div>
                        <div class="text-left">
                            <p class="text-green font-bold">${formatCurrency(e.salary)}</p>
                            <p class="text-xs text-gray">ئايلىق</p>
                        </div>
                    </div>
                    <div class="grid grid-3 gap-2 mt-3" style="border-top:1px solid rgba(255,255,255,0.1);padding-top:12px;">
                        <button onclick="editEmployee('${e.id}')" class="btn text-xs" style="background:rgba(99,102,241,0.2);color:#6366f1;">✏️</button>
                        <button onclick="paySalary('${e.id}')" class="btn text-xs" style="background:rgba(16,185,129,0.2);color:#10b981;">💰 مائاش</button>
                        <button onclick="deleteEmployee('${e.id}')" class="btn text-xs btn-danger">🗑️</button>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">خىزمەتچى يوق</p>';
        }
        function openEmployeeModal() { document.getElementById('employeeId').value = ''; document.getElementById('employeeForm').reset(); openModal('employeeModal'); }
        function editEmployee(id) {
            const e = data.employees.find(x => x.id === id);
            document.getElementById('employeeId').value = e.id;
            document.getElementById('employeeName').value = e.name;
            document.getElementById('employeePhone').value = e.phone || '';
            document.getElementById('employeeRole').value = e.role;
            document.getElementById('employeeSalary').value = e.salary;
            openModal('employeeModal');
        }
        function deleteEmployee(id) { if (confirm('ئۆچۈرەمسىز؟')) { data.employees = data.employees.filter(e => e.id !== id); save('employees'); loadEmployees(); toast('ئۆچۈرۈلدى!'); } }
        function handleEmployeeSubmit(e) {
            e.preventDefault();
            const id = document.getElementById('employeeId').value;
            const item = { id: id || genId(), name: document.getElementById('employeeName').value, phone: document.getElementById('employeePhone').value, role: document.getElementById('employeeRole').value, salary: parseFloat(document.getElementById('employeeSalary').value) };
            if (id) { const i = data.employees.findIndex(x => x.id === id); data.employees[i] = item; }
            else data.employees.push(item);
            save('employees'); closeModal('employeeModal'); loadEmployees(); toast('ساقلاندى!');
        }
        function paySalary(id) {
            const e = data.employees.find(x => x.id === id);
            if (!e) return;
            if (e.salary > data.cashboxes.usd) { toast('دوللار ساندۇقىدا پۇل يېتەرلىك ئەمەس!', '❌'); return; }
            if (!confirm(`${e.name} غا ${formatCurrency(e.salary)} مائاش بەرەمسىز؟`)) return;
            data.expenses.push({ id: genId(), category: 'salary', description: `${e.name} - ${roleNames[e.role]}`, amount: e.salary, createdAt: new Date().toISOString() });
            data.cashboxes.usd -= e.salary;
            data.transactions.push({ id: genId(), type: 'out', cashbox: 'usd', amount: e.salary, note: `مائاش: ${e.name}`, person: 'سېستىما', createdAt: new Date().toISOString() });
            save('expenses'); save('cashboxes'); save('transactions'); loadEmployees(); loadDashboard(); toast('مائاش تۆلەندى!');
        }
        
        // ==================== چىقىملار ====================
        function loadExpenses(cat = 'all') {
            document.getElementById('expenseFilter').innerHTML = 
                '<button class="filter-btn active" onclick="filterExpense(\'all\')">ھەممىسى</button>' +
                Object.entries(expenseCategories).map(([k, v]) => `<button class="filter-btn" onclick="filterExpense('${k}')">${v}</button>`).join('');
            
            let items = [...data.expenses].reverse();
            if (cat !== 'all') items = items.filter(e => e.category === cat);
            
            document.getElementById('expensesList').innerHTML = items.length ? items.map(e => `
                <div class="card mb-3">
                    <div class="flex items-center gap-3">
                        <span style="font-size:24px;">💸</span>
                        <div class="flex-1">
                            <h4 class="font-bold">${expenseCategories[e.category] || e.category}</h4>
                            <p class="text-xs text-gray">${e.description || ''}</p>
                            <p class="text-xs text-gray">${new Date(e.createdAt).toLocaleDateString()}</p>
                        </div>
                        <span class="text-red font-bold">${formatCurrency(e.amount)}</span>
                    </div>
                    <button onclick="deleteExpense('${e.id}')" class="btn text-xs btn-danger mt-2" style="width:100%;">🗑️ ئۆچۈر</button>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">چىقىم يوق</p>';
        }
        function filterExpense(cat) {
            document.querySelectorAll('#expenseFilter .filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            loadExpenses(cat);
        }
        function openExpenseModal() { openModal('expenseModal'); }
        function deleteExpense(id) { if (confirm('ئۆچۈرەمسىز؟')) { data.expenses = data.expenses.filter(e => e.id !== id); save('expenses'); loadExpenses(); loadDashboard(); toast('ئۆچۈرۈلدى!'); } }
        function handleExpenseSubmit(e) {
            e.preventDefault();
            const amount = parseFloat(document.getElementById('expenseAmount').value);
            if (amount > data.cashboxes.usd) { toast('دوللار ساندۇقىدا پۇل يوق!', '❌'); return; }
            const item = { id: genId(), category: document.getElementById('expenseCategory').value, description: document.getElementById('expenseDesc').value, amount, createdAt: new Date().toISOString() };
            data.expenses.push(item);
            data.cashboxes.usd -= amount;
            data.transactions.push({ id: genId(), type: 'out', cashbox: 'usd', amount, note: `چىقىم: ${expenseCategories[item.category]}`, person: 'سېستىما', createdAt: new Date().toISOString() });
            save('expenses'); save('cashboxes'); save('transactions'); closeModal('expenseModal'); loadExpenses(); loadDashboard(); toast('چىقىم قوشۇلدى!');
        }
        
        // ==================== ساندۇق ====================
        function loadCashbox() {
            document.getElementById('usdBalance').textContent = formatCurrency(data.cashboxes.usd || 0);
            document.getElementById('tryBalance').textContent = formatCurrency(data.cashboxes.try || 0, 'try');
            document.getElementById('cashBalance').textContent = formatCurrency(data.cashboxes.cash || 0);
            const total = (data.cashboxes.usd || 0) + (data.cashboxes.cash || 0) + ((data.cashboxes.try || 0) / exchangeRate);
            document.getElementById('totalBalance').textContent = formatCurrency(total);
            
            document.getElementById('transactionsList').innerHTML = data.transactions.length ? [...data.transactions].reverse().slice(0, 20).map(t => `
                <div class="card p-2 mb-2" style="border-right:3px solid ${t.type === 'in' ? '#10b981' : t.type === 'out' ? '#ef4444' : '#3b82f6'};">
                    <div class="flex items-center justify-between">
                        <div>
                            <span class="text-sm font-bold">${t.type === 'in' ? '➕' : t.type === 'out' ? '➖' : '🔄'} ${t.note || ''}</span>
                            <p class="text-xs text-gray">${t.person || ''} | ${new Date(t.createdAt).toLocaleString()}</p>
                        </div>
                        <span class="font-bold ${t.type === 'in' ? 'text-green' : t.type === 'out' ? 'text-red' : 'text-blue'}">${t.cashbox === 'try' ? formatCurrency(t.amount, 'try') : formatCurrency(t.amount)}</span>
                    </div>
                </div>
            `).join('') : '<p class="text-center text-gray p-4">مەشغۇلات يوق</p>';
        }
        
        function openCashboxModal(type, action) {
            document.getElementById('cashboxType').value = type;
            document.getElementById('cashboxAction').value = action;
            document.getElementById('cashboxModalTitle').textContent = `${type === 'usd' ? '💵 دوللار' : type === 'try' ? '💴 لىرا' : '💰 شامكاش'} - ${action === 'in' ? 'كىرىم' : 'چىقىم'}`;
            document.getElementById('cashboxPerson').innerHTML = '<option value="">تاللاڭ...</option>' + data.employees.map(e => `<option value="${e.name}">${e.name}</option>`).join('');
            openModal('cashboxModal');
        }
        
        function handleCashboxSubmit(e) {
            e.preventDefault();
            const type = document.getElementById('cashboxType').value;
            const action = document.getElementById('cashboxAction').value;
            const amount = parseFloat(document.getElementById('cashboxAmount').value);
            if (action === 'out' && amount > data.cashboxes[type]) { toast('پۇل يېتەرلىك ئەمەس!', '❌'); return; }
            if (action === 'in') data.cashboxes[type] += amount;
            else data.cashboxes[type] -= amount;
            data.transactions.push({ id: genId(), type: action, cashbox: type, amount, note: document.getElementById('cashboxNote').value, person: document.getElementById('cashboxPerson').value, createdAt: new Date().toISOString() });
            save('cashboxes'); save('transactions'); closeModal('cashboxModal'); loadCashbox(); toast(action === 'in' ? 'كىرىم قوشۇلدى!' : 'چىقىم قوشۇلدى!');
        }
        
        function openTransferModal(from) {
            document.getElementById('transferFrom').value = from;
            const names = { usd: '💵 دوللار', try: '💴 لىرا', cash: '💰 شامكاش' };
            document.getElementById('transferFromDisplay').innerHTML = `${names[from]} (${from === 'try' ? formatCurrency(data.cashboxes[from], 'try') : formatCurrency(data.cashboxes[from])})`;
            document.getElementById('transferTo').innerHTML = Object.keys(names).filter(k => k !== from).map(k => `<option value="${k}">${names[k]}</option>`).join('');
            document.getElementById('transferRate').textContent = `1$ = ${exchangeRate}₺`;
            document.getElementById('transferResult').textContent = '';
            openModal('transferModal');
        }
        
        function calcTransferResult() {
            const from = document.getElementById('transferFrom').value;
            const to = document.getElementById('transferTo').value;
            const amount = parseFloat(document.getElementById('transferAmount').value) || 0;
            let result = amount;
            if (from === 'try' && to !== 'try') result = amount / exchangeRate;
            else if (from !== 'try' && to === 'try') result = amount * exchangeRate;
            document.getElementById('transferResult').textContent = `${from === 'try' ? '₺' : '$'}${amount.toFixed(2)} → ${to === 'try' ? '₺' : '$'}${result.toFixed(2)}`;
        }
        
        function handleTransferSubmit(e) {
            e.preventDefault();
            const from = document.getElementById('transferFrom').value;
            const to = document.getElementById('transferTo').value;
            const amount = parseFloat(document.getElementById('transferAmount').value);
            if (amount > data.cashboxes[from]) { toast('پۇل يېتەرلىك ئەمەس!', '❌'); return; }
            let converted = amount;
            if (from === 'try' && to !== 'try') converted = amount / exchangeRate;
            else if (from !== 'try' && to === 'try') converted = amount * exchangeRate;
            data.cashboxes[from] -= amount;
            data.cashboxes[to] += converted;
            data.transactions.push({ id: genId(), type: 'transfer', from, to, amount, converted, note: document.getElementById('transferNote').value, person: '', createdAt: new Date().toISOString() });
            save('cashboxes'); save('transactions'); closeModal('transferModal'); loadCashbox(); toast('يۆتكەلدى!');
        }
        
        function exportTransactions() {
            const blob = new Blob([JSON.stringify(data.transactions, null, 2)], { type: 'application/json' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `transactions_${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            toast('چۈشۈرۈلدى!');
        }
        
        // ==================== تەڭشەكلەر ====================
        function loadSettings() {
            // تەڭشەكلەرنى يۈكلەش
            if (data.settings.name) document.getElementById('settingName').value = data.settings.name;
            if (data.settings.phone) document.getElementById('settingPhone').value = data.settings.phone;
            if (data.settings.address) document.getElementById('settingAddress').value = data.settings.address;
            document.getElementById('exchangeRate').value = exchangeRate;
            
            // ساقلاش بوشلۇقى
            updateStorageInfo();
        }
        
        async function updateStorageInfo() {
            try {
                if (navigator.storage && navigator.storage.estimate) {
                    const estimate = await navigator.storage.estimate();
                    const used = estimate.usage || 0;
                    const quota = estimate.quota || 100 * 1024 * 1024;
                    const mb = (used / 1024 / 1024).toFixed(2);
                    const percent = (used / quota * 100).toFixed(1);
                    document.getElementById('storageInfoDetail').textContent = mb + ' MB / ' + (quota / 1024 / 1024).toFixed(0) + ' MB';
                    document.getElementById('storageBarDetail').style.width = percent + '%';
                } else {
                    const bytes = new Blob([JSON.stringify(data)]).size;
                    const mb = (bytes / 1024 / 1024).toFixed(2);
                    document.getElementById('storageInfoDetail').textContent = mb + ' MB / 100 MB';
                    document.getElementById('storageBarDetail').style.width = Math.min(bytes / 1024 / 1024, 100) + '%';
                }
            } catch (error) {
                document.getElementById('storageInfoDetail').textContent = 'ھېسابلانمىدى';
            }
        }
        
        // ==================== دوكلات ====================
        function loadReports() { generateReport(); }
        function generateReport() {
            const from = document.getElementById('reportFrom').value;
            const to = document.getElementById('reportTo').value;
            let orders = data.orders.filter(o => o.status === 'completed');
            let expenses = [...data.expenses];
            if (from) { orders = orders.filter(o => new Date(o.createdAt) >= new Date(from)); expenses = expenses.filter(e => new Date(e.createdAt) >= new Date(from)); }
            if (to) { orders = orders.filter(o => new Date(o.createdAt) <= new Date(to + 'T23:59:59')); expenses = expenses.filter(e => new Date(e.createdAt) <= new Date(to + 'T23:59:59')); }
            const rev = orders.reduce((s, o) => s + o.total, 0);
            const exp = expenses.reduce((s, e) => s + e.amount, 0);
            document.getElementById('reportRevenue').textContent = formatCurrency(rev);
            document.getElementById('reportExpense').textContent = formatCurrency(exp);
            document.getElementById('reportProfit').textContent = formatCurrency(rev - exp);
        }
        function exportReport() {
            const report = { orders: data.orders, expenses: data.expenses, cashboxes: data.cashboxes, date: new Date().toISOString() };
            const blob = new Blob([JSON.stringify(report, null, 2)], { type: 'application/json' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `report_${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            toast('چۈشۈرۈلدى!');
        }
        
        // ==================== تەڭشەك ====================
        async function saveSettings() {
            data.settings = { 
                name: document.getElementById('settingName').value, 
                phone: document.getElementById('settingPhone').value, 
                address: document.getElementById('settingAddress').value 
            };
            exchangeRate = parseFloat(document.getElementById('exchangeRate').value);
            await saveToStore('settings', { key: 'main', value: data.settings });
            await saveToStore('settings', { key: 'exchangeRate', value: exchangeRate });
            toast('ساقلاندى!');
        }
        
        function backupData() {
            const backupObj = {
                ...data,
                exchangeRate,
                backupDate: new Date().toISOString(),
                version: '2.0-IndexedDB'
            };
            const blob = new Blob([JSON.stringify(backupObj, null, 2)], { type: 'application/json' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `restaurant_backup_${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            toast('زاپاسلاندى! 💾');
        }
        
        async function restoreData(input) {
            const file = input.files[0];
            if (!file) return;
            
            const reader = new FileReader();
            reader.onload = async (e) => {
                try {
                    const restored = JSON.parse(e.target.result);
                    
                    // ھەر بىر store غا ساقلاش
                    const stores = ['menu', 'orders', 'tables', 'rooms', 'employees', 'expenses', 'transactions'];
                    
                    for (const storeName of stores) {
                        if (restored[storeName] && Array.isArray(restored[storeName])) {
                            // كونىلارنى ئۆچۈرۈش
                            for (const item of data[storeName]) {
                                await deleteFromStore(storeName, item.id);
                            }
                            // يېڭىلارنى قوشۇش
                            for (const item of restored[storeName]) {
                                await saveToStore(storeName, item);
                            }
                            data[storeName] = restored[storeName];
                        }
                    }
                    
                    // ساندۇق ۋە تەڭشەك
                    if (restored.cashboxes) {
                        data.cashboxes = restored.cashboxes;
                        await saveToStore('cashboxes', { key: 'main', value: data.cashboxes });
                    }
                    if (restored.settings) {
                        data.settings = restored.settings;
                        await saveToStore('settings', { key: 'main', value: data.settings });
                    }
                    if (restored.exchangeRate) {
                        exchangeRate = restored.exchangeRate;
                        await saveToStore('settings', { key: 'exchangeRate', value: exchangeRate });
                    }
                    
                    toast('ئەسلىگە كەلتۈرۈلدى! ✅');
                    setTimeout(() => location.reload(), 1000);
                } catch (error) {
                    console.error('ئەسلىگە كەلتۈرۈش خاتالىقى:', error);
                    toast('خاتالىق! ❌', '❌');
                }
            };
            reader.readAsText(file);
        }
        
        async function clearAllData() {
            if (!confirm('ھەممە مەلۇماتنى ئۆچۈرەمسىز؟ بۇ مەشغۇلاتنى قايتۇرغىلى بولمايدۇ!')) return;
            if (!confirm('راستىنلا ئىشەنچىڭىز بارمۇ؟')) return;
            
            try {
                // IndexedDB نى ئۆچۈرۈش
                indexedDB.deleteDatabase(DB_NAME);
                // LocalStorage نى ئۆچۈرۈش
                localStorage.clear();
                toast('ھەممىسى ئۆچۈرۈلدى!');
                setTimeout(() => location.reload(), 1000);
            } catch (error) {
                console.error('ئۆچۈرۈش خاتالىقى:', error);
                localStorage.clear();
                location.reload();
            }
        }
        
        // ==================== خېرىدار زاكاز بېتى ====================
        let customerCart = [];
        let customerSelectedLocation = null;
        
        function loadCustomerPage() {
            // رېستوران نامى
            if (data.settings.name) {
                document.getElementById('customerRestaurantName').textContent = data.settings.name;
            }
            
            // 1-قەدەمگە قايتىش
            document.getElementById('customerStep1').style.display = 'block';
            document.getElementById('customerStep2').style.display = 'none';
            document.getElementById('customerStep3').style.display = 'none';
            customerSelectedLocation = null;
            customerCart = [];
            
            // ئۈستەللەرنى يۈكلەش
            document.getElementById('customerTablesGrid').innerHTML = data.tables.map(t => `
                <div class="location-card-luxury ${t.status}" onclick="selectCustomerLocation('table', '${t.id}')">
                    <span class="icon">${t.status === 'occupied' ? '🔴' : '🪑'}</span>
                    <div class="font-bold">${t.number}</div>
                    <div class="text-xs text-gray">${t.capacity} ئورۇن</div>
                </div>
            `).join('');
            
            // ئايرىمخانىلارنى يۈكلەش
            document.getElementById('customerRoomsGrid').innerHTML = data.rooms.map(r => `
                <div class="location-card-luxury ${r.status === 'occupied' ? 'occupied' : 'free'}" onclick="selectCustomerLocation('room', '${r.id}')" style="padding:20px;">
                    <div class="flex items-center gap-3">
                        <span style="font-size:36px;" class="floating-icon-slow">${r.status === 'occupied' ? '🔒' : '🚪'}</span>
                        <div class="text-right flex-1">
                            <div class="font-bold text-lg">${r.name}</div>
                            <div class="text-xs text-gray">${r.capacity} ئورۇن</div>
                            ${r.price > 0 ? `<div class="text-xs text-yellow">سائەتلىك: ${formatCurrency(r.price)}</div>` : ''}
                        </div>
                    </div>
                </div>
            `).join('');
        }
        
        function selectCustomerLocation(type, id) {
            if (type === 'takeaway') {
                customerSelectedLocation = { type: 'takeaway', name: 'ئېلىپ كېتىش', icon: '🛍️' };
            } else if (type === 'table') {
                const t = data.tables.find(x => x.id === id);
                if (t.status === 'occupied') { 
                    toast('بۇ ئۈستەل ئىگىلەنگەن!', '❌'); 
                    return; 
                }
                customerSelectedLocation = { type: 'table', id, name: 'ئۈستەل ' + t.number, icon: '🪑' };
            } else if (type === 'room') {
                const r = data.rooms.find(x => x.id === id);
                if (r.status === 'occupied') { 
                    toast('بۇ خانا ئىگىلەنگەن!', '❌'); 
                    return; 
                }
                customerSelectedLocation = { type: 'room', id, name: r.name, icon: '🚪' };
            }
            
            goToCustomerStep2();
        }
        
        function goToCustomerStep2() {
            document.getElementById('customerStep1').style.display = 'none';
            document.getElementById('customerStep2').style.display = 'block';
            document.getElementById('customerSelectedIcon').textContent = customerSelectedLocation.icon;
            document.getElementById('customerSelectedName').textContent = customerSelectedLocation.name;
            
            // تۈر سۈزگۈچ
            document.getElementById('customerCategoryFilter').innerHTML = 
                '<button class="filter-btn active" onclick="filterCustomerMenu(\'all\')">✨ ھەممىسى</button>' +
                categories.map(c => `<button class="filter-btn" onclick="filterCustomerMenu('${c.id}')">${c.emoji} ${c.name}</button>`).join('');
            
            loadCustomerMenu('all');
        }
        
        function backToCustomerStep1() {
            document.getElementById('customerStep1').style.display = 'block';
            document.getElementById('customerStep2').style.display = 'none';
            document.getElementById('customerStep3').style.display = 'none';
            customerSelectedLocation = null;
            customerCart = [];
        }
        
        function filterCustomerMenu(cat) {
            document.querySelectorAll('#customerCategoryFilter .filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            loadCustomerMenu(cat);
        }
        
        function loadCustomerMenu(cat) {
            let items = data.menu.filter(m => m.available);
            if (cat !== 'all') items = items.filter(m => m.category === cat);
            
            const catColors = {
                lagmen: 'rgba(245,158,11,0.5),rgba(234,88,12,0.4)',
                quruma: 'rgba(239,68,68,0.5),rgba(185,28,28,0.4)',
                qordaq: 'rgba(249,115,22,0.5),rgba(194,65,12,0.4)',
                kawap: 'rgba(239,68,68,0.5),rgba(220,38,38,0.4)',
                shorpa: 'rgba(234,179,8,0.5),rgba(202,138,4,0.4)',
                seleng: 'rgba(20,184,166,0.5),rgba(13,148,136,0.4)',
                nan: 'rgba(245,158,11,0.5),rgba(217,119,6,0.4)',
                aqash: 'rgba(148,163,184,0.5),rgba(100,116,139,0.4)',
                polo: 'rgba(249,115,22,0.5),rgba(234,88,12,0.4)',
                manta: 'rgba(244,114,182,0.5),rgba(219,39,119,0.4)',
                paket: 'rgba(16,185,129,0.5),rgba(5,150,105,0.4)',
                drink: 'rgba(6,182,212,0.5),rgba(8,145,178,0.4)',
                tea: 'rgba(34,197,94,0.5),rgba(22,163,74,0.4)'
            };
            
            document.getElementById('customerMenuGrid').innerHTML = items.map(m => `
                <div onclick="addToCustomerCart('${m.id}')" style="background:linear-gradient(145deg, ${catColors[m.category] || 'rgba(99,102,241,0.5),rgba(139,92,246,0.4)'});border-radius:16px;padding:16px 8px;text-align:center;cursor:pointer;border:1px solid rgba(255,255,255,0.1);box-shadow:0 8px 20px rgba(0,0,0,0.3);transition:all 0.3s;">
                    <div style="font-size:40px;margin-bottom:8px;filter:drop-shadow(0 4px 8px rgba(0,0,0,0.3));animation:floatSlow 3s ease-in-out infinite;">${getCatEmoji(m.category)}</div>
                    <div style="font-size:13px;font-weight:bold;margin-bottom:8px;color:white;text-shadow:0 2px 4px rgba(0,0,0,0.4);">${m.name}</div>
                    <div style="background:rgba(0,0,0,0.5);padding:6px 14px;border-radius:14px;display:inline-block;">
                        <span style="color:#10b981;font-weight:bold;font-size:14px;">${formatCurrency(m.price)}</span>
                    </div>
                </div>
            `).join('');
        }
        
        function addToCustomerCart(id) {
            const item = data.menu.find(m => m.id === id);
            if (!item) return;
            
            const existing = customerCart.find(c => c.id === id);
            if (existing) {
                existing.qty++;
            } else {
                customerCart.push({ ...item, qty: 1 });
            }
            
            updateCustomerCartUI();
            toast(item.name + ' قوشۇلدى!', '✅');
        }
        
        function updateCustomerCartUI() {
            const section = document.getElementById('customerStep3');
            
            if (customerCart.length === 0) {
                section.style.display = 'none';
                return;
            }
            
            section.style.display = 'block';
            
            const total = customerCart.reduce((s, c) => s + c.price * c.qty, 0);
            const count = customerCart.reduce((s, c) => s + c.qty, 0);
            
            document.getElementById('customerCartCount').textContent = count;
            document.getElementById('customerCartTotal').textContent = formatCurrency(total);
            
            document.getElementById('customerCartItems').innerHTML = customerCart.map(c => `
                <div class="flex items-center justify-between p-2 mb-2" style="background:rgba(255,255,255,0.05);border-radius:10px;">
                    <div class="flex items-center gap-2">
                        <span>${getCatEmoji(c.category)}</span>
                        <div>
                            <div class="font-bold text-sm">${c.name}</div>
                            <div class="text-xs text-green">${formatCurrency(c.price)}</div>
                        </div>
                    </div>
                    <div class="flex items-center gap-2">
                        <button onclick="updateCustomerCartQty('${c.id}', -1)" class="qty-btn minus">−</button>
                        <span class="font-bold">${c.qty}</span>
                        <button onclick="updateCustomerCartQty('${c.id}', 1)" class="qty-btn plus">+</button>
                    </div>
                </div>
            `).join('');
        }
        
        function updateCustomerCartQty(id, delta) {
            const item = customerCart.find(c => c.id === id);
            if (!item) return;
            
            item.qty += delta;
            if (item.qty <= 0) {
                customerCart = customerCart.filter(c => c.id !== id);
            }
            
            updateCustomerCartUI();
        }
        
        function clearCustomerCart() {
            customerCart = [];
            updateCustomerCartUI();
            toast('سېۋەت تازىلاندى', '🗑️');
        }
        
        async function submitCustomerOrder() {
            if (customerCart.length === 0) {
                toast('سېۋەت بوش!', '❌');
                return;
            }
            
            if (!customerSelectedLocation) {
                toast('ئورۇن تاللاڭ!', '❌');
                return;
            }
            
            const customerName = document.getElementById('customerName').value || 'خېرىدار';
            const note = document.getElementById('customerNote').value || '';
            
            const total = customerCart.reduce((s, c) => s + c.price * c.qty, 0);
            
            const order = {
                id: genId(),
                items: customerCart.map(c => ({ id: c.id, name: c.name, qty: c.qty, price: c.price })),
                total,
                location: customerSelectedLocation.name,
                locationType: customerSelectedLocation.type,
                locationId: customerSelectedLocation.id || '',
                customer: customerName,
                employee: '',
                employeeId: '',
                paymentMethod: '',
                paymentName: 'تۆلەنمىگەن',
                note: note,
                status: 'pending',
                source: 'customer',
                createdAt: new Date().toISOString()
            };
            
            data.orders.push(order);
            await saveItem('orders', order);
            
            // ئاگاھلاندۇرۇش
            playNotificationSound();
            
            customerCart = [];
            updateCustomerCartUI();
            document.getElementById('customerNote').value = '';
            
            toast('زاكاز يوللاندى! ساقلاڭ...', '✅');
            
            // زاكاز يوللانغانلىق كۆرسىتىش
            showCustomerOrderSuccess(order);
        }
        
        function showCustomerOrderSuccess(order) {
            const modal = document.createElement('div');
            modal.className = 'modal active';
            modal.id = 'customerSuccessModal';
            modal.innerHTML = `
                <div class="modal-overlay" onclick="closeCustomerSuccessModal()"></div>
                <div class="modal-content" style="text-align:center;">
                    <div class="modal-handle"></div>
                    <div style="font-size:80px;margin-bottom:16px;" class="floating-icon">✅</div>
                    <h2 class="text-2xl font-bold text-green mb-3">زاكاز قوبۇل قىلىندى!</h2>
                    <p class="text-gray mb-4">زاكاز نومۇرى: <strong>#${order.id.slice(-6)}</strong></p>
                    <div class="card mb-4" style="text-align:right;">
                        <p><strong>ئورنى:</strong> ${order.location}</p>
                        <p><strong>تائام سانى:</strong> ${order.items.reduce((s, i) => s + i.qty, 0)}</p>
                        <p><strong>جەمئىي:</strong> <span class="text-green font-bold">${formatCurrency(order.total)}</span></p>
                        ${order.note ? `<p><strong>ئېسكەرتىش:</strong> ${order.note}</p>` : ''}
                    </div>
                    <p class="text-sm text-gray mb-4">⏳ تائىمىڭىز تەييارلىنىۋاتىدۇ...</p>
                    <button onclick="closeCustomerSuccessModal()" class="btn btn-primary btn-3d glow-effect" style="width:100%;">👍 ماقۇل</button>
                </div>
            `;
            document.body.appendChild(modal);
        }
        
        function closeCustomerSuccessModal() {
            const modal = document.getElementById('customerSuccessModal');
            if (modal) modal.remove();
            // 1-قەدەمگە قايتىش
            loadCustomerPage();
        }
        
        function playNotificationSound() {
            // ئاددىي beep ئاۋازى
            try {
                const audioContext = new (window.AudioContext || window.webkitAudioContext)();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                
                oscillator.frequency.value = 800;
                oscillator.type = 'sine';
                gainNode.gain.value = 0.3;
                
                oscillator.start();
                setTimeout(() => {
                    oscillator.stop();
                }, 200);
                
                // ئىككىنچى beep
                setTimeout(() => {
                    const osc2 = audioContext.createOscillator();
                    const gain2 = audioContext.createGain();
                    osc2.connect(gain2);
                    gain2.connect(audioContext.destination);
                    osc2.frequency.value = 1000;
                    osc2.type = 'sine';
                    gain2.gain.value = 0.3;
                    osc2.start();
                    setTimeout(() => osc2.stop(), 200);
                }, 250);
            } catch (e) {
                console.log('ئاۋاز چىقىرالمىدى');
            }
        }
        
        // خېرىدار زاكازلىرىنى تەكشۈرۈش (باشقۇرغۇچى ئۈچۈن)
        function checkNewCustomerOrders() {
            const pendingOrders = data.orders.filter(o => o.source === 'customer' && o.status === 'pending');
            
            if (pendingOrders.length > 0) {
                // يېڭى زاكاز بار
                const badge = document.querySelector('#newOrderBadge');
                if (badge) {
                    badge.textContent = pendingOrders.length;
                    badge.style.display = 'block';
                }
            }
        }
        
        // ھەر 5 سېكۇنتتا تەكشۈرۈش
        setInterval(checkNewCustomerOrders, 5000);
        
        // خېرىدار ھالىتىنى ئېچىش
        function openCustomerMode() {
            toggleSidebar(false);
            
            // ئۈستى header نى يوشۇرۇش
            document.querySelector('.header').style.display = 'none';
            
            // ئاستىقى يول باشلاشنى يوشۇرۇش
            document.querySelector('.bottom-nav').style.display = 'none';
            
            // خېرىدار بېتىنى كۆرسىتىش
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById('page-customer').classList.add('active');
            document.getElementById('page-customer').style.paddingBottom = '200px';
            
            loadCustomerPage();
        }
        
        // باشقۇرۇش ھالىتىگە قايتىش
        function exitCustomerMode() {
            document.querySelector('.header').style.display = 'block';
            document.querySelector('.bottom-nav').style.display = 'flex';
            showPage('dashboard');
        }
        
        // كونا زاكازلارنى ئارخىپلاش
        async function archiveOldOrders(days = 30) {
            const cutoffDate = new Date();
            cutoffDate.setDate(cutoffDate.getDate() - days);
            
            const oldOrders = data.orders.filter(o => new Date(o.createdAt) < cutoffDate);
            const newOrders = data.orders.filter(o => new Date(o.createdAt) >= cutoffDate);
            
            if (oldOrders.length === 0) {
                toast('ئارخىپلايدىغان كونا زاكاز يوق');
                return;
            }
            
            // ئارخىپ ھۆججىتى چۈشۈرۈش
            const blob = new Blob([JSON.stringify(oldOrders, null, 2)], { type: 'application/json' });
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = `orders_archive_${new Date().toISOString().split('T')[0]}.json`;
            a.click();
            
            // كونا زاكازلارنى ئۆچۈرۈش
            for (const order of oldOrders) {
                await deleteFromStore('orders', order.id);
            }
            data.orders = newOrders;
            
            toast(`${oldOrders.length} زاكاز ئارخىپلاندى! 📦`);
            updateStorage();
        }
    </script>
</body>
</html># kapitanim-studio
