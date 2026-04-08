<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Visio</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=VT323&family=IBM+Plex+Mono:wght@300;400&display=swap');

  *{margin:0;padding:0;box-sizing:border-box}
  ::selection{background:#6b3fa0;color:#e0d0f0}
  ::-webkit-scrollbar{width:4px}
  ::-webkit-scrollbar-track{background:#000}
  ::-webkit-scrollbar-thumb{background:#2a1a3a;border-radius:2px}
  ::-webkit-scrollbar-thumb:hover{background:#4a2a6a}
  html,body{height:100%}

  body{
    background-color:#0e0818;
    font-family:'IBM Plex Mono',monospace;
    color:#c0b0d0;
    display:flex;align-items:center;justify-content:center;
    position:relative;overflow:hidden;
  }

  body::before{
    content:'';position:fixed;inset:0;z-index:1000;pointer-events:none;opacity:0.055;
    background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-repeat:repeat;background-size:128px 128px;
  }
  body::after{
    content:'';position:fixed;inset:0;z-index:999;pointer-events:none;
    background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,0.07) 2px,rgba(0,0,0,0.07) 4px);
  }

  .ambient{
    position:fixed;inset:0;z-index:0;pointer-events:none;
    background:
      radial-gradient(ellipse 600px 400px at 50% 50%,rgba(75,30,120,0.14),transparent),
      radial-gradient(ellipse 300px 300px at 30% 60%,rgba(40,10,80,0.1),transparent),
      radial-gradient(ellipse 200px 500px at 70% 40%,rgba(90,40,130,0.07),transparent);
  }

  @keyframes flicker{0%,100%{opacity:1}92%{opacity:1}93%{opacity:0.86}94%{opacity:1}96%{opacity:0.91}97%{opacity:1}}
  @keyframes subtlePulse{0%,100%{opacity:0.4}50%{opacity:0.7}}
  @keyframes blink{0%,100%{opacity:1}50%{opacity:0}}
  @keyframes fadeIn{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}
  @keyframes fadeInFast{from{opacity:0}to{opacity:1}}
  @keyframes shakeX{0%,100%{transform:translateX(0)}20%{transform:translateX(-6px)}40%{transform:translateX(5px)}60%{transform:translateX(-4px)}80%{transform:translateX(3px)}}
  @keyframes logoRotate{0%{transform:rotate(0deg)}100%{transform:rotate(360deg)}}
  @keyframes logoPulse{0%,100%{filter:drop-shadow(0 0 8px rgba(107,63,160,0.15))}50%{filter:drop-shadow(0 0 20px rgba(107,63,160,0.35))}}
  @keyframes redPulse{0%,100%{color:#4a2030}50%{color:#7a3050}}
  @keyframes barPulse{0%,100%{opacity:0.3}50%{opacity:0.8}}
  @keyframes scanDown{0%{top:-2px}100%{top:100%}}
  @keyframes dotBlink{0%,100%{opacity:1}50%{opacity:0.3}}
  @keyframes typewriter{from{width:0}to{width:100%}}

  .terminal-wrapper{
    position:relative;z-index:10;
    width:90%;max-width:640px;
    height:82vh;max-height:600px;min-height:420px;
  }
  .terminal{
    width:100%;height:100%;
    background:#000000;
    border:1px solid rgba(107,63,160,0.25);
    display:flex;flex-direction:column;
    animation:flicker 8s infinite,fadeIn 1.2s ease-out;
    box-shadow:0 0 60px rgba(75,30,120,0.08),inset 0 0 80px rgba(0,0,0,0.5);
    overflow:hidden;position:relative;
  }
  .terminal::before{
    content:'';position:absolute;top:-1px;left:20%;right:20%;height:1px;
    background:linear-gradient(to right,transparent,rgba(107,63,160,0.4),transparent);z-index:2;
  }
  .terminal::after{
    content:'';position:absolute;bottom:-1px;left:10%;right:10%;height:1px;
    background:linear-gradient(to right,transparent,rgba(107,63,160,0.15),transparent);z-index:2;
  }

  .status-bar{
    flex-shrink:0;display:flex;justify-content:space-between;align-items:center;
    padding:14px 24px;border-bottom:1px solid rgba(107,63,160,0.08);
  }
  .status-left{display:flex;align-items:center;gap:10px}
  .status-dot{
    width:5px;height:5px;border-radius:50%;background:#3a1a5a;
    animation:subtlePulse 3s ease-in-out infinite;transition:background 0.5s ease;
  }
  .status-text{
    font-size:9px;color:#2a1a3a;letter-spacing:2px;text-transform:uppercase;
    transition:color 0.5s ease;
  }
  .status-cursor{font-size:9px;color:#6b3fa0;animation:blink 1.2s step-end infinite}

  .terminal-content{
    flex:1;overflow-y:auto;padding:32px 32px 28px;scrollbar-gutter:stable;
  }

  .page{display:none;animation:fadeInFast 0.4s ease-out}
  .page.active{display:block}

  /* === HOME === */
  .title{
    font-family:'VT323',monospace;font-size:clamp(36px,8vw,56px);
    color:#d8c8e8;letter-spacing:8px;text-align:center;line-height:1;margin-bottom:20px;
    text-shadow:0 0 30px rgba(107,63,160,0.3);
  }
  .subtitle{
    text-align:center;font-size:14px;color:#7a6a8a;margin-bottom:8px;
    font-style:italic;animation:subtlePulse 4s ease-in-out infinite;
    letter-spacing:1px;
  }

  .logo-wrap{
    display:flex;justify-content:center;align-items:center;
    margin:24px auto 28px;position:relative;
  }
  .logo-ring{
    position:absolute;width:120px;height:120px;
    border:1px solid rgba(107,63,160,0.08);border-radius:50%;
    animation:logoRotate 60s linear infinite;
  }
  .logo-ring::before{
    content:'';position:absolute;top:-1px;left:50%;width:6px;height:6px;
    background:#6b3fa0;border-radius:50%;transform:translateX(-50%);
    box-shadow:0 0 6px rgba(107,63,160,0.5);
  }
  .logo-ring-inner{
    position:absolute;width:96px;height:96px;
    border:1px solid rgba(107,63,160,0.04);border-radius:50%;
    animation:logoRotate 45s linear infinite reverse;
  }
  .logo-ring-inner::before{
    content:'';position:absolute;bottom:-1px;left:50%;width:4px;height:4px;
    background:rgba(107,63,160,0.4);border-radius:50%;transform:translateX(-50%);
  }
  .logo-img{
    width:88px;height:88px;object-fit:contain;
    opacity:0.7;position:relative;z-index:1;
    filter:drop-shadow(0 0 12px rgba(107,63,160,0.2));
    animation:logoPulse 5s ease-in-out infinite;
    transition:opacity 0.5s ease,filter 0.5s ease;
  }
  .logo-img:hover{
    opacity:1;
    filter:drop-shadow(0 0 25px rgba(107,63,160,0.45)) brightness(1.1);
  }

  .sep{border:none;height:1px;background:linear-gradient(to right,transparent,rgba(107,63,160,0.2),transparent);margin:0 0 28px}

  .nav{
    display:flex;flex-wrap:wrap;justify-content:center;gap:4px 0;
    margin-bottom:32px;font-size:12px;letter-spacing:0.5px;
  }
  .nav a{
    color:#6b5a80;text-decoration:none;padding:4px 10px;
    transition:color 0.3s ease,text-shadow 0.3s ease;position:relative;cursor:pointer;
  }
  .nav a::after{
    content:'|';position:absolute;right:-4px;color:rgba(107,63,160,0.15);pointer-events:none;
  }
  .nav a:last-child::after{display:none}
  .nav a:hover{color:#c0a8e0;text-shadow:0 0 8px rgba(107,63,160,0.4)}
  .nav a.clickable{color:#8a6aaa}
  .nav a.clickable:hover{color:#c0a8e0;text-shadow:0 0 12px rgba(107,63,160,0.5)}

  .visibility{
    text-align:center;font-size:11px;color:#3a2a4a;margin-bottom:28px;
    transition:color 0.5s ease;line-height:1.8;
  }
  .visibility .go-visible{
    color:#4a3a5a;cursor:pointer;text-decoration:none;
    border-bottom:1px dashed rgba(107,63,160,0.2);transition:color 0.3s ease,border-color 0.3s ease;
  }
  .visibility .go-visible:hover{color:#9b7ac0;border-bottom-color:rgba(107,63,160,0.5)}

  .copyright{text-align:center;font-size:10px;color:#2a1a3a;letter-spacing:0.5px}
  .fan-credit{
    text-align:center;font-size:9px;color:#1f1528;letter-spacing:0.5px;
    margin-top:8px;transition:color 0.4s ease;
  }
  .fan-credit a{color:#1f1528;text-decoration:none;transition:color 0.4s ease}
  .fan-credit:hover,.fan-credit:hover a{color:#4a3a5a}

  /* === SHARED COMPONENTS === */
  .page-header{text-align:center;margin-bottom:32px}
  .page-header .label{font-size:9px;letter-spacing:3px;text-transform:uppercase;color:#3a2a4a;margin-bottom:8px}
  .page-header h2{
    font-family:'VT323',monospace;font-size:24px;color:#c0a8e0;letter-spacing:2px;
    text-shadow:0 0 15px rgba(107,63,160,0.25);
  }
  .page-header .sub{text-align:center;font-size:11px;color:#4a3a5a;margin-top:10px;line-height:1.6}

  .signal-bars{display:flex;align-items:center;justify-content:center;gap:3px;margin-top:12px}
  .signal-bars .bar{
    width:3px;background:#6b3fa0;border-radius:1px;animation:barPulse 1.5s ease-in-out infinite;
  }
  .signal-bars .bar:nth-child(1){height:6px;animation-delay:0s}
  .signal-bars .bar:nth-child(2){height:10px;animation-delay:0.15s}
  .signal-bars .bar:nth-child(3){height:14px;animation-delay:0.3s}
  .signal-bars .bar:nth-child(4){height:10px;animation-delay:0.45s}
  .signal-bars .bar:nth-child(5){height:6px;animation-delay:0.6s}

  .section{margin-bottom:28px}
  .section-title{
    font-size:9px;letter-spacing:2.5px;text-transform:uppercase;color:#4a3a5a;
    margin-bottom:12px;padding-bottom:6px;border-bottom:1px solid rgba(107,63,160,0.08);
  }
  .bio-text{font-size:12px;line-height:1.9;color:#7a6a8a}
  .bio-text strong{color:#9b7ac0;font-weight:400}

  .stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
  .stat-item{
    background:rgba(107,63,160,0.03);border:1px solid rgba(107,63,160,0.06);
    padding:12px 14px;transition:border-color 0.3s ease;
  }
  .stat-item:hover{border-color:rgba(107,63,160,0.15)}
  .stat-value{font-family:'VT323',monospace;font-size:20px;color:#8a6aaa;letter-spacing:1px}
  .stat-label{font-size:9px;color:#3a2a3a;letter-spacing:1px;text-transform:uppercase;margin-top:2px}

  .tags{display:flex;flex-wrap:wrap;gap:6px}
  .tag{
    font-size:10px;color:#5a4a6a;border:1px solid rgba(107,63,160,0.1);padding:4px 10px;
    letter-spacing:0.5px;transition:color 0.3s ease,border-color 0.3s ease,background 0.3s ease;cursor:default;
  }
  .tag:hover{color:#9b7ac0;border-color:rgba(107,63,160,0.25);background:rgba(107,63,160,0.04)}

  .timeline{position:relative;padding-left:16px}
  .timeline::before{
    content:'';position:absolute;left:3px;top:4px;bottom:4px;width:1px;
    background:rgba(107,63,160,0.12);
  }
  .timeline-item{position:relative;padding-bottom:16px;padding-left:12px}
  .timeline-item:last-child{padding-bottom:0}
  .timeline-item::before{
    content:'';position:absolute;left:-13px;top:6px;width:5px;height:5px;
    border-radius:50%;background:#3a1a5a;border:1px solid rgba(107,63,160,0.25);
  }
  .timeline-year{font-family:'VT323',monospace;font-size:14px;color:#6b3fa0;letter-spacing:1px}
  .timeline-desc{font-size:11px;color:#5a4a6a;margin-top:2px;line-height:1.6}

  .quote-block{
    text-align:center;padding:20px;border:1px solid rgba(107,63,160,0.06);
    background:rgba(107,63,160,0.015);margin-bottom:28px;
  }
  .quote-block blockquote{font-style:italic;font-size:12px;color:#6b5a80;line-height:1.8}
  .quote-block cite{
    display:block;margin-top:8px;font-size:9px;color:#3a2a3a;
    letter-spacing:1.5px;text-transform:uppercase;font-style:normal;
  }

  .back-btn{
    display:inline-flex;align-items:center;gap:8px;
    font-family:'IBM Plex Mono',monospace;font-size:11px;color:#4a3a5a;
    background:none;border:1px solid rgba(107,63,160,0.1);padding:8px 18px;
    cursor:pointer;letter-spacing:1px;text-transform:uppercase;
    transition:color 0.3s ease,border-color 0.3s ease,background 0.3s ease;
  }
  .back-btn:hover{color:#9b7ac0;border-color:rgba(107,63,160,0.3);background:rgba(107,63,160,0.04)}
  .back-btn .arrow{font-size:14px;transition:transform 0.3s ease}
  .back-btn:hover .arrow{transform:translateX(-3px)}

  .page-footer{
    display:flex;justify-content:space-between;align-items:center;
    margin-top:32px;padding-top:16px;border-top:1px solid rgba(107,63,160,0.06);
  }
  .page-footer .coord{font-size:9px;color:#2a1a3a;letter-spacing:1px}

  /* === LOGIN === */
  .form-group{margin-bottom:20px}
  .form-label{display:block;font-size:9px;letter-spacing:2px;text-transform:uppercase;color:#3a2a4a;margin-bottom:8px}
  .form-input{
    width:100%;background:rgba(107,63,160,0.03);border:1px solid rgba(107,63,160,0.1);
    padding:10px 14px;font-family:'IBM Plex Mono',monospace;font-size:12px;color:#c0b0d0;
    outline:none;transition:border-color 0.3s ease,background 0.3s ease;letter-spacing:0.5px;
  }
  .form-input::placeholder{color:#2a1a3a}
  .form-input:focus{border-color:rgba(107,63,160,0.3);background:rgba(107,63,160,0.05)}
  .form-input.error{border-color:rgba(120,40,60,0.4);animation:shakeX 0.4s ease}
  .form-error{font-size:10px;color:#6a3040;margin-top:8px;line-height:1.5;min-height:16px;opacity:0;transition:opacity 0.3s ease}
  .form-error.visible{opacity:1}

  .login-btn{
    width:100%;background:rgba(107,63,160,0.06);border:1px solid rgba(107,63,160,0.15);
    padding:12px;font-family:'VT323',monospace;font-size:16px;color:#8a6aaa;
    letter-spacing:2px;cursor:pointer;transition:all 0.3s ease;margin-bottom:20px;
  }
  .login-btn:hover{background:rgba(107,63,160,0.1);border-color:rgba(107,63,160,0.3);color:#c0a8e0;text-shadow:0 0 10px rgba(107,63,160,0.3)}
  .login-btn:active{transform:scale(0.99)}

  .divider-line{
    text-align:center;font-size:9px;color:#2a1a3a;letter-spacing:2px;
    margin-bottom:20px;position:relative;
  }
  .divider-line::before,.divider-line::after{
    content:'';position:absolute;top:50%;width:calc(50% - 40px);height:1px;background:rgba(107,63,160,0.08);
  }
  .divider-line::before{left:0}
  .divider-line::after{right:0}

  .pseudo-link{
    text-align:center;font-size:12px;color:#4a3a5a;cursor:pointer;padding:8px;
    border:1px dashed rgba(107,63,160,0.08);transition:color 0.3s ease,border-color 0.3s ease,background 0.3s ease;
  }
  .pseudo-link:hover{color:#8a6aaa;border-color:rgba(107,63,160,0.2);background:rgba(107,63,160,0.02)}

  .protocol-dots{display:flex;align-items:center;justify-content:center;gap:8px;margin-top:14px}
  .protocol-dot{
    width:4px;height:4px;border-radius:50%;background:#2a1a3a;animation:subtlePulse 2s ease-in-out infinite;
  }
  .protocol-dot.on{background:#6b3fa0}
  .protocol-dot:nth-child(1){animation-delay:0s}
  .protocol-dot:nth-child(2){animation-delay:0.3s}
  .protocol-dot:nth-child(3){animation-delay:0.6s}

  /* === INVITE === */
  .invite-icon{
    font-size:48px;margin-bottom:24px;opacity:0.15;font-family:'VT323',monospace;
    color:#6b3fa0;letter-spacing:4px;text-align:center;
  }
  .invite-title{
    font-family:'VT323',monospace;font-size:22px;color:#8a5a7a;letter-spacing:2px;
    margin-bottom:20px;text-align:center;text-shadow:0 0 15px rgba(120,40,80,0.15);
  }
  .invite-msg{font-size:12px;color:#5a4a6a;line-height:2;max-width:380px;margin:0 auto 12px;text-align:center}
  .invite-msg strong{color:#7a4a6a;font-weight:400}
  .invite-note{font-size:10px;color:#3a2a3a;line-height:1.8;margin-top:24px;padding-top:20px;border-top:1px solid rgba(107,63,160,0.06);text-align:center}
  .invite-stats{display:flex;justify-content:center;gap:32px;margin-top:28px;padding:16px;border:1px solid rgba(107,63,160,0.05);background:rgba(107,63,160,0.015)}
  .invite-stat-val{font-family:'VT323',monospace;font-size:20px;color:#4a3a5a;letter-spacing:1px;text-align:center}
  .invite-stat-label{font-size:8px;color:#2a1a3a;letter-spacing:1.5px;text-transform:uppercase;margin-top:2px;text-align:center}

  /* === HELP === */
  .help-empty{text-align:center;padding:40px 0}
  .help-empty .icon{font-size:36px;color:#2a1a3a;margin-bottom:20px;font-family:'VT323',monospace;letter-spacing:4px}
  .help-empty .msg{font-size:12px;color:#3a2a4a;line-height:2}
  .help-faq{margin-top:32px;text-align:left}
  .faq-item{
    border-bottom:1px solid rgba(107,63,160,0.05);padding:14px 0;cursor:pointer;
    transition:background 0.3s ease;
  }
  .faq-item:hover{background:rgba(107,63,160,0.015)}
  .faq-q{
    font-size:11px;color:#5a4a6a;letter-spacing:0.5px;display:flex;
    justify-content:space-between;align-items:center;
  }
  .faq-q .faq-arrow{
    font-size:10px;color:#2a1a3a;transition:transform 0.3s ease,color 0.3s ease;
  }
  .faq-item.open .faq-arrow{transform:rotate(90deg);color:#6b3fa0}
  .faq-a{
    max-height:0;overflow:hidden;opacity:0;font-size:11px;color:#4a3a5a;
    line-height:1.8;transition:max-height 0.4s ease,opacity 0.3s ease,margin 0.3s ease;margin-top:0;
  }
  .faq-item.open .faq-a{max-height:120px;opacity:1;margin-top:10px}

  /* === TRACKLIST === */
  .tracklist-controls{display:flex;gap:8px;margin-bottom:20px;flex-wrap:wrap}
  .tl-filter{
    font-family:'IBM Plex Mono',monospace;font-size:9px;letter-spacing:1.5px;text-transform:uppercase;
    color:#4a3a5a;background:none;border:1px solid rgba(107,63,160,0.08);padding:6px 14px;
    cursor:pointer;transition:all 0.3s ease;
  }
  .tl-filter:hover,.tl-filter.active{color:#8a6aaa;border-color:rgba(107,63,160,0.25);background:rgba(107,63,160,0.04)}

  .track-item{
    display:flex;justify-content:space-between;align-items:center;
    padding:10px 14px;border-bottom:1px solid rgba(107,63,160,0.04);
    transition:background 0.3s ease;cursor:default;
  }
  .track-item:hover{background:rgba(107,63,160,0.025)}
  .track-info{display:flex;align-items:center;gap:12px}
  .track-num{font-family:'VT323',monospace;font-size:14px;color:#2a1a3a;width:28px;text-align:right}
  .track-meta .track-name{font-size:12px;color:#7a6a8a;letter-spacing:0.5px;transition:color 0.3s ease}
  .track-item:hover .track-name{color:#c0a8e0}
  .track-meta .track-tag{font-size:9px;color:#2a1a3a;letter-spacing:1px;text-transform:uppercase;margin-top:2px}
  .track-dur{font-family:'VT323',monospace;font-size:14px;color:#3a2a4a;letter-spacing:1px}
  .track-corrupted .track-name{color:#4a2a3a;text-decoration:line-through}
  .track-hidden .track-name{color:#1a1a2a;filter:blur(3px);user-select:none}

  .tracklist-more{text-align:center;margin-top:16px}
  .tracklist-more .tl-btn{
    font-size:10px;color:#3a2a4a;background:none;border:1px solid rgba(107,63,160,0.06);
    padding:8px 20px;cursor:pointer;letter-spacing:1px;transition:all 0.3s ease;
  }
  .tracklist-more .tl-btn:hover{color:#6b3fa0;border-color:rgba(107,63,160,0.2)}

  /* === GUESTBOOK === */
  .gb-entry{
    border-bottom:1px solid rgba(107,63,160,0.05);padding:16px 0;
    transition:background 0.3s ease;
  }
  .gb-entry:hover{background:rgba(107,63,160,0.01)}
  .gb-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
  .gb-user{font-family:'VT323',monospace;font-size:14px;color:#6b3fa0;letter-spacing:1px}
  .gb-date{font-size:9px;color:#2a1a3a;letter-spacing:1px}
  .gb-msg{font-size:12px;color:#5a4a6a;line-height:1.8}
  .gb-empty{font-style:italic;color:#3a2a4a}
  .gb-blank{color:#2a1a2a;font-size:11px;letter-spacing:1px}

  .gb-form{margin-top:28px;padding-top:20px;border-top:1px solid rgba(107,63,160,0.06)}
  .gb-form .form-input{margin-bottom:12px}
  .gb-submit{
    font-family:'VT323',monospace;font-size:14px;color:#5a4a6a;background:none;
    border:1px solid rgba(107,63,160,0.1);padding:10px 24px;cursor:pointer;
    letter-spacing:2px;transition:all 0.3s ease;
  }
  .gb-submit:hover{color:#8a6aaa;border-color:rgba(107,63,160,0.25);background:rgba(107,63,160,0.04)}
  .gb-form-note{font-size:9px;color:#2a1a3a;margin-top:10px;line-height:1.6}

  /* === DONATE === */
  .donate-amounts{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:24px}
  .donate-amt{
    text-align:center;padding:14px 8px;
    border:1px solid rgba(107,63,160,0.06);background:rgba(107,63,160,0.015);
    cursor:pointer;transition:all 0.3s ease;
  }
  .donate-amt:hover{border-color:rgba(107,63,160,0.2);background:rgba(107,63,160,0.04)}
  .donate-amt .val{font-family:'VT323',monospace;font-size:20px;color:#6b3fa0;letter-spacing:1px}
  .donate-amt .lbl{font-size:8px;color:#2a1a3a;letter-spacing:1.5px;text-transform:uppercase;margin-top:4px}
  .donate-msg{
    text-align:center;font-size:11px;color:#4a3a5a;line-height:1.8;
    padding:16px;border:1px solid rgba(107,63,160,0.04);background:rgba(107,63,160,0.01);margin-bottom:24px;
  }
  .donate-msg strong{color:#6b3fa0;font-weight:400}
  .donate-warning{
    text-align:center;font-size:10px;color:#3a2a3a;line-height:1.8;
    padding:12px;border:1px dashed rgba(107,63,160,0.06);margin-bottom:20px;
  }
  .donate-total{text-align:center;margin-top:24px}
  .donate-total .big{font-family:'VT323',monospace;font-size:28px;color:#3a2a4a;letter-spacing:2px}
  .donate-total .sub{font-size:9px;color:#2a1a3a;letter-spacing:1px;margin-top:4px}

  /* === NOISE IN GOD === */
  .noise-display{
    position:relative;height:180px;border:1px solid rgba(107,63,160,0.06);
    background:#030108;margin-bottom:24px;overflow:hidden;cursor:crosshair;
  }
  .noise-display canvas{width:100%;height:100%;display:block}
  .noise-overlay-text{
    position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
    font-family:'VT323',monospace;font-size:18px;color:rgba(107,63,160,0.15);
    letter-spacing:6px;pointer-events:none;text-transform:uppercase;
    animation:subtlePulse 3s ease-in-out infinite;
  }
  .noise-log{margin-top:8px}
  .noise-entry{
    font-size:10px;color:#3a2a4a;line-height:2;padding-left:12px;
    border-left:1px solid rgba(107,63,160,0.06);margin-bottom:4px;
  }
  .noise-entry .ts{color:#2a1a2a;margin-right:8px;font-size:9px}
  .noise-entry .val{color:#4a3a5a}
  .noise-controls{
    display:flex;justify-content:space-between;align-items:center;margin-top:20px;
    padding-top:16px;border-top:1px solid rgba(107,63,160,0.04);
  }
  .noise-btn{
    font-family:'IBM Plex Mono',monospace;font-size:9px;letter-spacing:1.5px;text-transform:uppercase;
    color:#3a2a4a;background:none;border:1px solid rgba(107,63,160,0.06);
    padding:6px 14px;cursor:pointer;transition:all 0.3s ease;
  }
  .noise-btn:hover{color:#6b3fa0;border-color:rgba(107,63,160,0.2)}
  .noise-btn.active{color:#6b3fa0;border-color:rgba(107,63,160,0.2);background:rgba(107,63,160,0.04)}
  .noise-freq{font-family:'VT323',monospace;font-size:14px;color:#2a1a3a;letter-spacing:1px}

  /* === VISIBLE PAGE === */
  .vis-alert{
    text-align:center;padding:16px;
    border:1px solid rgba(107,63,160,0.12);
    background:rgba(107,63,160,0.03);
    margin-bottom:28px;
    position:relative;
    overflow:hidden;
  }
  .vis-alert::after{
    content:'';position:absolute;left:0;right:0;height:1px;
    background:rgba(107,63,160,0.3);
    animation:scanDown 3s linear infinite;
  }
  .vis-alert .alert-text{
    font-family:'VT323',monospace;font-size:15px;
    color:#8a6aaa;letter-spacing:2px;
    animation:dotBlink 2s ease-in-out infinite;
  }

  .vis-fingerprint{
    background:rgba(107,63,160,0.02);
    border:1px solid rgba(107,63,160,0.06);
    padding:16px;margin-bottom:12px;
    font-family:'VT323',monospace;font-size:13px;
    color:#4a3a5a;letter-spacing:1px;line-height:1.8;
    word-break:break-all;
  }
  .vis-fingerprint .fp-label{
    font-family:'IBM Plex Mono',monospace;
    font-size:8px;letter-spacing:2px;text-transform:uppercase;
    color:#2a1a3a;margin-bottom:8px;
  }
  .vis-fingerprint .fp-hash{
    color:#6b3fa0;opacity:0.7;
  }

  .vis-warning{
    text-align:center;font-size:11px;color:#5a3040;
    line-height:1.9;padding:16px;
    border:1px dashed rgba(120,40,60,0.1);
    margin-bottom:24px;
  }
  .vis-warning strong{color:#7a4050;font-weight:400}

  .vis-log{margin-bottom:24px}
  .vis-log-entry{
    font-size:10px;line-height:2;padding-left:12px;
    border-left:1px solid rgba(107,63,160,0.08);margin-bottom:2px;
    color:#3a2a4a;
  }
  .vis-log-entry .ts{color:#2a1a2a;margin-right:8px;font-size:9px}
  .vis-log-entry .val{color:#5a4a6a}

  .vis-now-playing{
    text-align:center;padding:20px;
    border:1px solid rgba(107,63,160,0.06);
    background:rgba(107,63,160,0.015);margin-bottom:24px;
  }
  .vis-now-playing .np-label{
    font-size:8px;letter-spacing:2px;text-transform:uppercase;
    color:#2a1a3a;margin-bottom:10px;
  }
  .vis-now-playing .np-title{
    font-family:'VT323',monospace;font-size:16px;
    color:#8a6aaa;letter-spacing:1px;
  }
  .vis-now-playing .np-dur{
    font-size:10px;color:#3a2a4a;margin-top:4px;
  }
  .vis-now-playing .np-bar{
    margin:12px auto 0;width:80%;height:2px;
    background:rgba(107,63,160,0.06);border-radius:1px;
    overflow:hidden;
  }
  .vis-now-playing .np-bar-fill{
    height:100%;width:34%;
    background:linear-gradient(to right,rgba(107,63,160,0.3),rgba(107,63,160,0.08));
    animation:npProgress 8s linear infinite;
  }
  @keyframes npProgress{0%{width:34%}100%{width:100%}}

  .vis-stats-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}
  .vis-stat{
    text-align:center;padding:14px 8px;
    border:1px solid rgba(107,63,160,0.04);
    background:rgba(107,63,160,0.01);
  }
  .vis-stat .vs-val{
    font-family:'VT323',monospace;font-size:18px;
    color:#6b3fa0;letter-spacing:1px;
  }
  .vis-stat .vs-label{
    font-size:7px;color:#2a1a3a;letter-spacing:1.5px;
    text-transform:uppercase;margin-top:4px;
  }

  .toast{
    position:fixed;bottom:24px;left:50%;
    transform:translateX(-50%) translateY(20px);
    background:rgba(10,6,18,0.95);border:1px solid rgba(107,63,160,0.2);
    color:#7a6a8a;font-family:'IBM Plex Mono',monospace;font-size:11px;
    padding:10px 24px;z-index:2000;opacity:0;pointer-events:none;
    transition:opacity 0.4s ease,transform 0.4s ease;letter-spacing:0.5px;
  }
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0)}

  @media(max-width:480px){
    .terminal-content{padding:24px 18px 20px}
    .nav{font-size:10px;gap:2px 0}
    .nav a{padding:3px 6px}
    .nav a::after{right:-2px}
    .stats-grid,.donate-amounts{grid-template-columns:1fr}
    .vis-stats-grid{grid-template-columns:1fr 1fr}
    .page-footer{flex-direction:column;gap:10px;align-items:flex-start}
    .invite-stats{flex-direction:column;gap:12px}
    .track-item{padding:8px 10px}
    .noise-display{height:140px}
    .logo-img{width:72px;height:72px}
    .logo-ring{width:100px;height:100px}
    .logo-ring-inner{width:80px;height:80px}
  }
</style>
</head>
<body>

<div class="ambient"></div>

<div class="terminal-wrapper">
  <div class="terminal">
    <div class="status-bar">
      <div class="status-left">
        <div class="status-dot" id="statusDot"></div>
        <span class="status-text" id="statusText">session active</span>
      </div>
      <span class="status-cursor">█</span>
    </div>

    <div class="terminal-content">

      <!-- ═══ HOME ═══ -->
      <div class="page active" id="homePage">
        <div class="title">Visio</div>
        <div class="subtitle">ras.fx</div>
        <div class="logo-wrap">
          <div class="logo-ring"></div>
          <div class="logo-ring-inner"></div>
          <img class="logo-img" src="https://z-cdn-media.chatglm.cn/files/9407024d-74d2-4fb8-ad96-4a7604d72ccc.png?auth_key=1875689502-12c213a3ad544892ac5c12076e8b0af3-0-42afcbe4ac29fd50f462428c2fc9823e" alt="Visio">
        </div>
        <hr class="sep">
        <nav class="nav">
          <a class="clickable" data-nav="about">About Me</a>
          <a class="clickable" data-nav="login">Visio/Login</a>
          <a class="clickable" data-nav="help">Help</a>
          <a class="clickable" data-nav="tracklist">Tracklist Dir/etc</a>
          <a class="clickable" data-nav="guestbook">Guestbook</a>
          <a class="clickable" data-nav="donate">Donate</a>
          <a class="clickable" data-nav="noise">Noise in God</a>
        </nav>
        <div class="visibility">
          You are invisible. <a class="go-visible" href="#" id="goVisible" onclick="navigateTo('visible');return false">[Go visible]</a>
        </div>
        <hr class="sep">
        <div class="copyright">Copyright © 2013 - 2025 Visio</div>
        <div class="fan-credit">fan made page of <a href="https://fauux.neocities.org/" target="_blank" rel="noopener">fauux.neocities.org</a></div>
      </div>

      <!-- ═══ ABOUT ═══ -->
      <div class="page" id="aboutPage">
        <div class="page-header">
          <div class="label">signal broadcast</div>
          <h2>About Visio</h2>
          <div class="signal-bars"><div class="bar"></div><div class="bar"></div><div class="bar"></div><div class="bar"></div><div class="bar"></div></div>
        </div>
        <div class="quote-block">
          <blockquote>"I don't make music for people. I make it for the space between them."</blockquote>
          <cite>— unknown transmission, 2014</cite>
        </div>
        <div class="section">
          <div class="section-title">Identity</div>
          <div class="bio-text">No real name. No face. Just <strong>Visio</strong>.<br><br>I've been running this archive from a room you'll never find, curating sounds that exist in the margins — field recordings of empty parking structures at 3am, corrupted MIDI files recovered from dead hard drives, loops that repeat until they become something else entirely.<br><br>Visio has been online since 2013. It will outlast me.</div>
        </div>
        <div class="section">
          <div class="section-title">Transmit Stats</div>
          <div class="stats-grid">
            <div class="stat-item"><div class="stat-value">3,247</div><div class="stat-label">Tracks Archived</div></div>
            <div class="stat-item"><div class="stat-value">12</div><div class="stat-label">Years Online</div></div>
            <div class="stat-item"><div class="stat-value">0</div><div class="stat-label">Social Media</div></div>
            <div class="stat-item"><div class="stat-value">∞</div><div class="stat-label">Noise Generated</div></div>
          </div>
        </div>
        <div class="section">
          <div class="section-title">Frequencies</div>
          <div class="tags">
            <span class="tag">ambient</span><span class="tag">dark ambient</span><span class="tag">drone</span>
            <span class="tag">field recording</span><span class="tag">vaporwave</span><span class="tag">glitch</span>
            <span class="tag">noise</span><span class="tag">lo-fi</span><span class="tag">industrial</span>
            <span class="tag">post-internet</span><span class="tag">tape loops</span><span class="tag">silence</span>
          </div>
        </div>
        <div class="section">
          <div class="section-title">Signal Log</div>
          <div class="timeline">
            <div class="timeline-item"><div class="timeline-year">2013</div><div class="timeline-desc">First transmission. Visio goes live on a shared host that no longer exists.</div></div>
            <div class="timeline-item"><div class="timeline-year">2015</div><div class="timeline-desc">Archive passes 500 tracks. Guestbook receives its last real message.</div></div>
            <div class="timeline-item"><div class="timeline-year">2018</div><div class="timeline-desc">Migrated to current server. All original filenames preserved.</div></div>
            <div class="timeline-item"><div class="timeline-year">2020</div><div class="timeline-desc">Traffic drops to near zero. Uploads increase to 3 per week.</div></div>
            <div class="timeline-item"><div class="timeline-year">2023</div><div class="timeline-desc">3,000th track archived. Celebrated alone.</div></div>
            <div class="timeline-item"><div class="timeline-year">2025</div><div class="timeline-desc">Still here. Still listening. Still sad.</div></div>
          </div>
        </div>
        <div class="section">
          <div class="section-title">Contact Protocol</div>
          <div class="bio-text">There is no DM. No email form. No Discord.<br><br>If you need to reach me, leave something in the <strong>guestbook</strong>. I read every entry, even the blank ones.<br><br>If you want to submit a track, upload it somewhere and burn the link into a <strong>comment on any page</strong>. I'll find it.</div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Back to Visio</span></button>
          <span class="coord">47.3769° N, 8.5417° E</span>
        </div>
      </div>

      <!-- ═══ LOGIN ═══ -->
      <div class="page" id="loginPage">
        <div class="page-header">
          <div class="label">authentication required</div>
          <h2>Visio / Login</h2>
          <div class="sub">Access the Visio network.<br>You know who you are.</div>
          <div class="protocol-dots"><div class="protocol-dot on"></div><div class="protocol-dot on"></div><div class="protocol-dot"></div></div>
        </div>
        <form id="loginForm" onsubmit="return handleLogin(event)">
          <div class="form-group">
            <label class="form-label" for="loginUser">Handle</label>
            <input class="form-input" type="text" id="loginUser" placeholder="your_handle" autocomplete="off" spellcheck="false">
            <div class="form-error" id="loginUserError"></div>
          </div>
          <div class="form-group">
            <label class="form-label" for="loginPass">Passphrase</label>
            <input class="form-input" type="password" id="loginPass" placeholder="••••••••">
            <div class="form-error" id="loginPassError"></div>
          </div>
          <button type="submit" class="login-btn">CONNECT</button>
        </form>
        <div class="divider-line">or</div>
        <div class="pseudo-link" onclick="navigateTo('invite')">create new identity</div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Disconnect</span></button>
          <span class="coord">tls 1.3 // encrypted</span>
        </div>
      </div>

      <!-- ═══ INVITE ═══ -->
      <div class="page" id="invitePage">
        <div style="padding-top:30px">
          <div class="invite-icon">[ × ]</div>
          <div class="invite-title">Registration Closed</div>
          <div class="invite-msg">
            New identities require an <strong>invitation</strong> from an existing Visio member.<br><br>
            Invitations are not given. They are <strong>earned</strong> — or they find you.<br><br>
            Don't ask how. If you don't know, you're not ready.
          </div>
          <div class="invite-note">Last invitation issued: 847 days ago<br>Total active identities: 14</div>
          <div class="invite-stats">
            <div><div class="invite-stat-val">3</div><div class="invite-stat-label">Invites This Year</div></div>
            <div><div class="invite-stat-val">14</div><div class="invite-stat-label">Active Users</div></div>
            <div><div class="invite-stat-val">0</div><div class="invite-stat-label">Pending Requests</div></div>
          </div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('login')"><span class="arrow">←</span><span>Back to Login</span></button>
          <span class="coord">access denied</span>
        </div>
      </div>

      <!-- ═══ HELP ═══ -->
      <div class="page" id="helpPage">
        <div class="page-header">
          <div class="label">null</div>
          <h2>Help</h2>
          <div class="sub">There is nothing here that can help you.</div>
        </div>
        <div class="help-empty">
          <div class="icon">[ ? ]</div>
          <div class="msg">This section exists because every website has a help section.<br>It doesn't mean this website will help you.</div>
        </div>
        <div class="help-faq">
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>How do I download tracks?</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">You don't. Tracks are streamed through Visio. If you were meant to have them, you already would.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>Can I submit my own music?</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">Leave a link in the guestbook. If I find it worthy, it will appear in the archive. You will not be notified.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>The site looks broken / is loading slowly.</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">It's not broken. You're just not used to something that doesn't want to be fast. Sit with it.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>Who runs this site?</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">Someone who stopped answering this question in 2016. See: About Visio.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>How do I contact the admin?</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">You can't. But the admin can find you. That's how it works here.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>I found a bug / broken link.</span><span class="faq-arrow">→</span></div>
            <div class="faq-a">That's a feature. Everything on this site is exactly as intended, including the things that seem wrong.</div>
          </div>
          <div class="faq-item" onclick="toggleFaq(this)">
            <div class="faq-q"><span>Is this site abandoned?</span><span class="faq-arrow">→</span></div>
            <div class="faq-a" style="animation:redPulse 4s ease-in-out infinite">No. It is maintained. It is watched. It is waiting.</div>
          </div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Give Up</span></button>
          <span class="coord">help_index: 0 results</span>
        </div>
      </div>

      <!-- ═══ TRACKLIST ═══ -->
      <div class="page" id="tracklistPage">
        <div class="page-header">
          <div class="label">archive index</div>
          <h2>Tracklist Dir/etc</h2>
          <div class="sub">3,247 files. Most are silence. Some are not.</div>
        </div>
        <div class="section">
          <div class="section-title">Filter</div>
          <div class="tracklist-controls">
            <button class="tl-filter active" onclick="filterTracks('all',this)">All</button>
            <button class="tl-filter" onclick="filterTracks('ambient',this)">Ambient</button>
            <button class="tl-filter" onclick="filterTracks('drone',this)">Drone</button>
            <button class="tl-filter" onclick="filterTracks('field',this)">Field</button>
            <button class="tl-filter" onclick="filterTracks('glitch',this)">Glitch</button>
            <button class="tl-filter" onclick="filterTracks('corrupted',this)">Corrupted</button>
          </div>
        </div>
        <div id="tracklistContainer"></div>
        <div class="tracklist-more">
          <button class="tl-btn" onclick="showToast('you don\'t need to see the rest.')">show remaining 3,227 tracks</button>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Stop Listening</span></button>
          <span class="coord">3,247 files // 847gb</span>
        </div>
      </div>

      <!-- ═══ GUESTBOOK ═══ -->
      <div class="page" id="guestbookPage">
        <div class="page-header">
          <div class="label">transmission log</div>
          <h2>Guestbook</h2>
          <div class="sub">Leave something. Or leave nothing. Both are valid.</div>
        </div>
        <div id="guestbookEntries">
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">sleeper_0x</span><span class="gb-date">2019.11.03 — 03:17</span></div>
            <div class="gb-msg">found Visio through a dead link on a geocities mirror. been here every night since. thank you for track 2,847. it sounds like the room i can't go back to.</div>
          </div>
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">null.user</span><span class="gb-date">2018.06.22 — 01:44</span></div>
            <div class="gb-msg gb-empty"></div>
          </div>
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">drift://archive</span><span class="gb-date">2017.02.14 — 23:58</span></div>
            <div class="gb-msg">valentine's day. appropriate that i'm here. submitted a field recording of rain on a tin roof. if it doesn't get added, i understand. if it does, i won't know.</div>
          </div>
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">anonymous</span><span class="gb-date">2016.09.11 — 04:02</span></div>
            <div class="gb-msg gb-blank">[message deleted by user]</div>
          </div>
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">______</span><span class="gb-date">2015.12.25 — 00:00</span></div>
            <div class="gb-msg">christmas. the quietest day of the year. this is where i come to make it quieter.</div>
          </div>
          <div class="gb-entry">
            <div class="gb-head"><span class="gb-user">exiled_signal</span><span class="gb-date">2015.03.08 — unknown</span></div>
            <div class="gb-msg">i don't know how i found Visio. i don't know how to leave.</div>
          </div>
        </div>
        <div class="gb-form">
          <div class="section-title">Leave Transmission</div>
          <input class="form-input" type="text" id="gbName" placeholder="handle (optional)" autocomplete="off" spellcheck="false">
          <textarea class="form-input" id="gbMsg" placeholder="your message... (leave empty for silence)" rows="3" style="resize:none"></textarea>
          <button class="gb-submit" onclick="submitGuestbook()">TRANSMIT</button>
          <div class="gb-form-note">messages are not moderated. messages are not read aloud.<br>they simply exist here, like everything else.</div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Close Book</span></button>
          <span class="coord">6 entries since 2015</span>
        </div>
      </div>

      <!-- ═══ DONATE ═══ -->
      <div class="page" id="donatePage">
        <div class="page-header">
          <div class="label">sustenance protocol</div>
          <h2>Donate</h2>
          <div class="sub">Visio costs money to exist in the dark.</div>
        </div>
        <div class="donate-msg">
          Server costs: <strong>$4.20/month</strong><br>
          Domain renewal: <strong>$12.00/year</strong><br>
          Human cost: <strong>incalculable</strong>
        </div>
        <div class="section">
          <div class="section-title">Select Amount</div>
          <div class="donate-amounts">
            <div class="donate-amt" onclick="selectDonation(this,1)"><div class="val">$1</div><div class="lbl">a whisper</div></div>
            <div class="donate-amt" onclick="selectDonation(this,5)"><div class="val">$5</div><div class="lbl">a hum</div></div>
            <div class="donate-amt" onclick="selectDonation(this,20)"><div class="val">$20</div><div class="lbl">a signal</div></div>
            <div class="donate-amt" onclick="selectDonation(this,50)"><div class="val">$50</div><div class="lbl">a broadcast</div></div>
            <div class="donate-amt" onclick="selectDonation(this,100)"><div class="val">$100</div><div class="lbl">a frequency</div></div>
            <div class="donate-amt" onclick="selectDonation(this,0)"><div class="val">∞</div><div class="lbl">everything</div></div>
          </div>
        </div>
        <button class="login-btn" onclick="attemptDonate()" id="donateBtn" style="margin-bottom:16px">PROCEED TO NOTHING</button>
        <div class="donate-warning" id="donateWarning" style="display:none">
          payment processor not found.<br>Visio does not accept money from the outside world.<br>if you want to help, keep the archive alive by linking to it somewhere no one will look.
        </div>
        <div class="donate-total">
          <div class="big">$0.00</div>
          <div class="sub">total received since 2013</div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="navigateTo('home')"><span class="arrow">←</span><span>Keep Your Money</span></button>
          <span class="coord">btc: no // xmr: no // usd: no</span>
        </div>
      </div>

      <!-- ═══ NOISE IN GOD ═══ -->
      <div class="page" id="noisePage">
        <div class="page-header">
          <div class="label">experimental</div>
          <h2>Noise in God</h2>
          <div class="sub">An open channel. The signal speaks when it wants to.</div>
        </div>
        <div class="noise-display" id="noiseDisplay">
          <canvas id="noiseCanvas"></canvas>
          <div class="noise-overlay-text">listen</div>
        </div>
        <div class="section">
          <div class="section-title">Transmission Log</div>
          <div class="noise-log" id="noiseLog">
            <div class="noise-entry"><span class="ts">00:00:00</span><span class="val">channel opened</span></div>
          </div>
        </div>
        <div class="noise-controls">
          <div style="display:flex;gap:8px">
            <button class="noise-btn active" id="noiseToggle" onclick="toggleNoise()">RECEIVING</button>
            <button class="noise-btn" onclick="clearNoiseLog()">CLEAR LOG</button>
          </div>
          <div class="noise-freq" id="noiseFreq">0.000 Hz</div>
        </div>
        <div class="page-footer">
          <button class="back-btn" onclick="stopNoiseAndGoHome()"><span class="arrow">←</span><span>Close Channel</span></button>
          <span class="coord">entropy: rising</span>
        </div>
      </div>

      <!-- ═══ VISIBLE ═══ -->
      <div class="page" id="visiblePage">
        <div class="vis-alert">
          <div class="alert-text">● YOU ARE NOW VISIBLE</div>
        </div>

        <div class="vis-warning">
          <strong>Warning:</strong> Your presence is now logged. Your signal fingerprint has been recorded. Other visitors may be able to detect you. Going invisible again will not erase this record.
        </div>

        <div class="section">
          <div class="section-title">Signal Fingerprint</div>
          <div class="vis-fingerprint">
            <div class="fp-label">visitor hash</div>
            <div class="fp-hash" id="visitorHash"></div>
          </div>
        </div>

        <div class="section">
          <div class="section-title">Detection Metrics</div>
          <div class="vis-stats-grid">
            <div class="vis-stat"><div class="vs-val" id="visTime">--:--</div><div class="vs-label">Time Visible</div></div>
            <div class="vis-stat"><div class="vs-val" id="visPings">0</div><div class="vs-label">Signal Pings</div></div>
            <div class="vis-stat"><div class="vs-val" id="visOthers">0</div><div class="vs-label">Others Visible</div></div>
          </div>
        </div>

        <div class="section">
          <div class="section-title">Now Playing</div>
          <div class="vis-now-playing">
            <div class="np-label">currently streaming</div>
            <div class="np-title">untitled_04.wav</div>
            <div class="np-dur">3:47</div>
            <div class="np-bar"><div class="np-bar-fill"></div></div>
          </div>
        </div>

        <div class="section">
          <div class="section-title">Visibility Log</div>
          <div class="vis-log" id="visLog">
            <div class="vis-log-entry"><span class="ts">00:00</span><span class="val">visibility state changed: invisible → visible</span></div>
            <div class="vis-log-entry"><span class="ts">00:00</span><span class="val">signal fingerprint generated</span></div>
            <div class="vis-log-entry"><span class="ts">00:00</span><span class="val">broadcasting presence to network</span></div>
          </div>
        </div>

        <div class="page-footer">
          <button class="back-btn" onclick="goInvisible()"><span class="arrow">←</span><span>Go Invisible</span></button>
          <span class="coord" id="visCoord">visible since: --</span>
        </div>
      </div>

    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
  const pages = {
    home: document.getElementById('homePage'),
    about: document.getElementById('aboutPage'),
    login: document.getElementById('loginPage'),
    invite: document.getElementById('invitePage'),
    help: document.getElementById('helpPage'),
    tracklist: document.getElementById('tracklistPage'),
    guestbook: document.getElementById('guestbookPage'),
    donate: document.getElementById('donatePage'),
    noise: document.getElementById('noisePage'),
    visible: document.getElementById('visiblePage')
  };

  const statusText = document.getElementById('statusText');
  const statusDot = document.getElementById('statusDot');
  const terminalContent = document.querySelector('.terminal-content');

  const statusMap = {
    home: () => ({ text: 'session active', color: '#3a1a5a' }),
    about: () => ({ text: 'about_visio.exe', color: '#6b3fa0' }),
    login: () => ({ text: 'auth handshake', color: '#5a3a7a' }),
    invite: () => ({ text: 'access denied', color: '#5a2a3a' }),
    help: () => ({ text: 'null', color: '#2a1a3a' }),
    tracklist: () => ({ text: 'reading index...', color: '#5a4a6a' }),
    guestbook: () => ({ text: 'transmission log', color: '#4a3a6a' }),
    donate: () => ({ text: 'no payment gateway', color: '#3a2a3a' }),
    noise: () => ({ text: 'receiving...', color: '#6b3fa0' }),
    visible: () => ({ text: 'user detected', color: '#6b3fa0' })
  };

  function navigateTo(key) {
    Object.values(pages).forEach(p => p.classList.remove('active'));
    pages[key].classList.add('active');
    terminalContent.scrollTop = 0;
    const s = statusMap[key]();
    statusText.textContent = s.text;
    statusDot.style.background = s.color;
    if (key === 'login') {
      document.querySelectorAll('.form-error').forEach(e => { e.classList.remove('visible'); e.textContent = ''; });
      document.querySelectorAll('#loginPage .form-input').forEach(i => i.classList.remove('error'));
    }
    if (key === 'noise') startNoise(); else stopNoise();
    if (key === 'visible') initVisiblePage();
  }

  document.querySelectorAll('[data-nav]').forEach(a => {
    a.addEventListener('click', e => { e.preventDefault(); navigateTo(a.dataset.nav); });
  });

  // === VISIBLE PAGE ===
  let visTimerInterval = null;
  let visPingInterval = null;
  let visSeconds = 0;
  let visPings = 0;

  function generateHash() {
    const chars = '0123456789abcdef';
    let hash = '';
    for (let i = 0; i < 32; i++) {
      if (i === 8 || i === 12 || i === 16 || i === 20) hash += ':';
      hash += chars[Math.floor(Math.random() * 16)];
    }
    return hash;
  }

  function initVisiblePage() {
    visSeconds = 0;
    visPings = 0;
    document.getElementById('visitorHash').textContent = generateHash();
    const now = new Date();
    const ts = `${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
    document.getElementById('visCoord').textContent = `visible since: ${ts}`;
    document.getElementById('visTime').textContent = '0:00';
    document.getElementById('visPings').textContent = '0';
    document.getElementById('visOthers').textContent = String(Math.floor(Math.random() * 3));

    clearInterval(visTimerInterval);
    clearInterval(visPingInterval);

    visTimerInterval = setInterval(() => {
      visSeconds++;
      const m = String(Math.floor(visSeconds / 60)).padStart(2, '0');
      const s = String(visSeconds % 60).padStart(2, '0');
      document.getElementById('visTime').textContent = `${m}:${s}`;
    }, 1000);

    visPingInterval = setInterval(() => {
      visPings++;
      document.getElementById('visPings').textContent = visPings;

      const m = String(Math.floor(visSeconds / 60)).padStart(2, '0');
      const s = String(visSeconds % 60).padStart(2, '0');
      const log = document.getElementById('visLog');
      const entry = document.createElement('div');
      entry.className = 'vis-log-entry';
      entry.style.animation = 'fadeInFast 0.3s ease-out';

      const msgs = [
        'heartbeat acknowledged by network',
        'presence broadcast sent',
        'signal strength: ' + (Math.random() * 0.3 + 0.1).toFixed(3),
        'location approximated: ' + (Math.random() * 180 - 90).toFixed(4) + '°, ' + (Math.random() * 360 - 180).toFixed(4) + '°',
        'fingerprint verified',
        'packet forwarded to ' + Math.floor(Math.random() * 14) + ' node(s)',
        'noise floor detected: ' + (Math.random() * -60 - 80).toFixed(1) + ' dBm',
        'visibility confirmed by remote',
        'latency: ' + (Math.random() * 200 + 50).toFixed(0) + 'ms',
        'another visitor may have seen you',
        'signal decay: negligible',
        'network trusts this fingerprint',
      ];
      const msg = msgs[Math.floor(Math.random() * msgs.length)];
      entry.innerHTML = `<span class="ts">${m}:${s}</span><span class="val">${msg}</span>`;
      log.appendChild(entry);
      log.parentElement.scrollTop = log.parentElement.scrollHeight;
      if (log.children.length > 30) log.removeChild(log.children[0]);

      if (Math.random() > 0.7) {
        const others = parseInt(document.getElementById('visOthers').textContent);
        document.getElementById('visOthers').textContent = String(others + (Math.random() > 0.5 ? 1 : -1));
      }
    }, 3000);
  }

  function goInvisible() {
    clearInterval(visTimerInterval);
    clearInterval(visPingInterval);
    const log = document.getElementById('visLog');
    const m = String(Math.floor(visSeconds / 60)).padStart(2, '0');
    const s = String(visSeconds % 60).padStart(2, '0');
    const entry = document.createElement('div');
    entry.className = 'vis-log-entry';
    entry.style.animation = 'fadeInFast 0.3s ease-out';
    entry.innerHTML = `<span class="ts">${m}:${s}</span><span class="val" style="color:#5a3040">visibility state changed: visible → invisible</span>`;
    log.appendChild(entry);
    showToast('you are invisible again. the record remains.');
    navigateTo('home');
  }

  // === TOAST ===
  let toastTimeout;
  function showToast(msg) {
    const t = document.getElementById('toast');
    t.textContent = '> ' + msg; t.classList.add('show');
    clearTimeout(toastTimeout);
    toastTimeout = setTimeout(() => t.classList.remove('show'), 2800);
  }

  // === LOGIN ===
  const loginMsgs = [
    'no account found with that handle.','connection terminated by remote host.',
    'incorrect passphrase. try again. or don\'t.','that handle doesn\'t exist in this network.',
    'authentication failed. who are you?','Visio rejects you.',
    'handle not recognized. this isn\'t your place.','wrong. try harder or leave.',
    'error: user not found. error: user not missed.','access denied. permanently.'
  ];
  function handleLogin(e) {
    e.preventDefault();
    const user = document.getElementById('loginUser'), pass = document.getElementById('loginPass');
    const userErr = document.getElementById('loginUserError'), passErr = document.getElementById('loginPassError');
    let hasError = false;
    userErr.classList.remove('visible'); passErr.classList.remove('visible');
    user.classList.remove('error'); pass.classList.remove('error');
    if (!user.value.trim()) { userErr.textContent = 'handle required.'; userErr.classList.add('visible'); user.classList.add('error'); hasError = true; }
    if (!pass.value.trim()) { passErr.textContent = 'passphrase required.'; passErr.classList.add('visible'); pass.classList.add('error'); hasError = true; }
    if (!hasError) {
      const msg = loginMsgs[Math.floor(Math.random() * loginMsgs.length)];
      userErr.textContent = msg; userErr.classList.add('visible');
      user.classList.add('error'); pass.classList.add('error');
      showToast(msg);
    }
    return false;
  }

  function toggleFaq(el) { el.classList.toggle('open'); }

  // === TRACKLIST ===
  const tracks = [
    { num: '2847', name: 'the room i can\'t go back to', dur: '11:23', tag: 'ambient', cls: '' },
    { num: '3190', name: 'rain_on_tin_roof_submission.wav', dur: '7:44', tag: 'field', cls: '' },
    { num: '3201', name: '██████████', dur: '0:00', tag: 'corrupted', cls: 'track-corrupted' },
    { num: '3210', name: 'elevator_d_shaft_resonance', dur: '14:02', tag: 'drone', cls: '' },
    { num: '3215', name: '[REDACTED]', dur: '3:47', tag: 'glitch', cls: 'track-hidden' },
    { num: '3221', name: 'untitled_04.wav', dur: '3:47', tag: 'ambient', cls: '' },
    { num: '3228', name: 'microwave_at_3am_final.wav', dur: '0:58', tag: 'field', cls: '' },
    { num: '3231', name: 'data_recovery_fragment_0091', dur: '2:14', tag: 'glitch', cls: '' },
    { num: '3235', name: 'silence_with_undercurrent', dur: '22:00', tag: 'drone', cls: '' },
    { num: '3239', name: 'corrupted_midi_hymn_v3', dur: '1:33', tag: 'corrupted', cls: 'track-corrupted' },
    { num: '3241', name: 'the_space_between_people', dur: '8:16', tag: 'ambient', cls: '' },
    { num: '3244', name: 'traffic_light_buzz_downtown', dur: '4:07', tag: 'field', cls: '' },
    { num: '3245', name: 'god_is_static', dur: '∞', tag: 'noise', cls: '' },
    { num: '3246', name: 'last_message_left_on_answering_machine', dur: '0:12', tag: 'ambient', cls: '' },
    { num: '3247', name: 'goodbye.wav', dur: '0:03', tag: 'ambient', cls: '' },
  ];
  function renderTracks(filter) {
    const c = document.getElementById('tracklistContainer');
    const filtered = filter === 'all' ? tracks : tracks.filter(t => t.tag === filter);
    c.innerHTML = filtered.map(t =>
      `<div class="track-item ${t.cls}"><div class="track-info"><span class="track-num">${t.num}</span><div class="track-meta"><div class="track-name">${t.name}</div><div class="track-tag">${t.tag}</div></div></div><span class="track-dur">${t.dur}</span></div>`
    ).join('');
  }
  function filterTracks(filter, btn) {
    document.querySelectorAll('.tl-filter').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    renderTracks(filter);
  }
  renderTracks('all');

  // === GUESTBOOK ===
  function submitGuestbook() {
    const name = document.getElementById('gbName').value.trim() || 'anonymous';
    const msg = document.getElementById('gbMsg').value.trim();
    const now = new Date();
    const date = `${now.getFullYear()}.${String(now.getMonth()+1).padStart(2,'0')}.${String(now.getDate()).padStart(2,'0')} — ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;
    const container = document.getElementById('guestbookEntries');
    const entry = document.createElement('div');
    entry.className = 'gb-entry';
    entry.style.animation = 'fadeInFast 0.6s ease-out';
    entry.innerHTML = `<div class="gb-head"><span class="gb-user">${name.replace(/</g,'&lt;')}</span><span class="gb-date">${date}</span></div><div class="gb-msg${msg ? '' : ' gb-empty'}">${msg ? msg.replace(/</g,'&lt;') : ''}</div>`;
    container.insertBefore(entry, container.firstChild);
    document.getElementById('gbName').value = '';
    document.getElementById('gbMsg').value = '';
    showToast(msg ? 'transmission received.' : 'silence recorded.');
  }

  // === DONATE ===
  let selectedAmount = 0;
  function selectDonation(el, amt) {
    document.querySelectorAll('.donate-amt').forEach(d => d.style.borderColor = 'rgba(107,63,160,0.06)');
    el.style.borderColor = 'rgba(107,63,160,0.3)';
    selectedAmount = amt;
    document.getElementById('donateWarning').style.display = 'none';
  }
  function attemptDonate() {
    if (selectedAmount === 0) { showToast('select an amount first.'); return; }
    document.getElementById('donateWarning').style.display = 'block';
    showToast('payment gateway not found.');
  }

  // === NOISE ===
  let noiseRunning = false, noiseAnimId = null, noiseSeconds = 0, noiseInterval = null;
  const noiseWords = [
    'signal fragment detected','interference pattern shifting','void resonance at 0.00 Hz',
    'carrier wave: absent','entropy spike: negligible','static梦见god','frequency drift: -∞',
    'silence is not empty','the wire hums backwards','buffer overflow in faith.dll',
    'packet loss: 100%','god is not responding','crc mismatch in reality.wav',
    'unknown protocol: LOVE','connection timed out (eternal)','heartbeat not found',
    'thermal noise at absolute zero','the signal knows your name','segmentation fault in soul',
    'routing through purgatory','tcp_handshake with nothing','ghost frequency: 0.001 Hz'
  ];
  function startNoise() {
    if (noiseRunning) return;
    noiseRunning = true;
    const canvas = document.getElementById('noiseCanvas');
    const ctx = canvas.getContext('2d');
    const display = document.getElementById('noiseDisplay');
    canvas.width = display.offsetWidth * 2;
    canvas.height = display.offsetHeight * 2;
    ctx.scale(2, 2);
    const w = display.offsetWidth, h = display.offsetHeight;
    const imgData = ctx.createImageData(w, h);
    const data = imgData.data;
    function draw() {
      for (let i = 0; i < data.length; i += 4) {
        const v = Math.random() * 18;
        data[i] = v * 0.5; data[i+1] = v * 0.2; data[i+2] = v; data[i+3] = 255;
      }
      ctx.putImageData(imgData, 0, 0);
      noiseAnimId = requestAnimationFrame(draw);
    }
    draw();
    noiseInterval = setInterval(() => {
      noiseSeconds++;
      document.getElementById('noiseFreq').textContent = (Math.random() * 0.01).toFixed(6) + ' Hz';
      if (Math.random() > 0.5) {
        const word = noiseWords[Math.floor(Math.random() * noiseWords.length)];
        const hh = String(Math.floor(noiseSeconds / 3600)).padStart(2, '0');
        const mm = String(Math.floor((noiseSeconds % 3600) / 60)).padStart(2, '0');
        const ss = String(noiseSeconds % 60).padStart(2, '0');
        const log = document.getElementById('noiseLog');
        const entry = document.createElement('div');
        entry.className = 'noise-entry';
        entry.style.animation = 'fadeInFast 0.3s ease-out';
        entry.innerHTML = `<span class="ts">${hh}:${mm}:${ss}</span><span class="val">${word}</span>`;
        log.appendChild(entry);
        log.parentElement.scrollTop = log.parentElement.scrollHeight;
        if (log.children.length > 50) log.removeChild(log.children[0]);
      }
    }, 1000);
    document.getElementById('noiseToggle').textContent = 'RECEIVING';
    document.getElementById('noiseToggle').classList.add('active');
  }
  function stopNoise() {
    noiseRunning = false;
    if (noiseAnimId) cancelAnimationFrame(noiseAnimId);
    if (noiseInterval) clearInterval(noiseInterval);
  }
  function toggleNoise() {
    const btn = document.getElementById('noiseToggle');
    if (noiseRunning) { stopNoise(); btn.textContent = 'PAUSED'; btn.classList.remove('active'); }
    else startNoise();
  }
  function clearNoiseLog() {
    document.getElementById('noiseLog').innerHTML = '<div class="noise-entry"><span class="ts">--:--:--</span><span class="val">log cleared</span></div>';
  }
  function stopNoiseAndGoHome() {
    stopNoise(); noiseSeconds = 0;
    document.getElementById('noiseFreq').textContent = '0.000 Hz';
    document.getElementById('noiseLog').innerHTML = '<div class="noise-entry"><span class="ts">00:00:00</span><span class="val">channel opened</span></div>';
    const btn = document.getElementById('noiseToggle');
    btn.textContent = 'RECEIVING'; btn.classList.add('active');
    navigateTo('home');
  }

  // === FLICKER ===
  const terminal = document.querySelector('.terminal');
  setInterval(() => {
    if (Math.random() > 0.97) {
      terminal.style.opacity = '0.92';
      setTimeout(() => { terminal.style.opacity = '1'; }, 80);
    }
  }, 3000);
</script>

</body>
</html>
