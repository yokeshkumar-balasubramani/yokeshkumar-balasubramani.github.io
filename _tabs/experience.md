---
title: Experience
icon: fas fa-briefcase
order: 5
---

<style>
.timeline-container {
  max-height: calc(100vh - 200px);
  overflow-y: auto;
  padding: 0 20px;
}

.timeline-container::-webkit-scrollbar {
  width: 4px;
}

.timeline-container::-webkit-scrollbar-track {
  background: transparent;
  border-radius: 2px;
}

.timeline-container::-webkit-scrollbar-thumb {
  background: #888;
  border-radius: 2px;
}

.timeline-container::-webkit-scrollbar-thumb:hover {
  background: #666;
}
.timeline {
  position: relative;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px 0;
}

.timeline::after {
  content: '';
  position: absolute;
  width: 4px;
  background: var(--border-color);
  top: 0;
  bottom: 0;
  left: 50%;
  margin-left: -2px;
  border-radius: 2px;
}

.timeline-item {
  padding: 10px 40px;
  position: relative;
  width: 50%;
  box-sizing: border-box;
}

.timeline-item::after {
  content: '';
  position: absolute;
  width: 20px;
  height: 20px;
  background: var(--link-color);
  border: 4px solid var(--bg-color);
  border-radius: 50%;
  top: 30px;
  right: -12px;
  z-index: 1;
}

.timeline-item:nth-child(even) {
  left: 50%;
}

.timeline-item:nth-child(even)::after {
  left: -12px;
}

.timeline-content {
  padding: 20px;
  background: var(--bg-color);
  border: 1px solid var(--border-color);
  border-radius: 8px;
  position: relative;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.timeline-content::after {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border: 10px solid transparent;
  border-right-color: var(--bg-color);
  top: 20px;
  right: -10px;
}

.timeline-item:nth-child(even) .timeline-content::after {
  border-right-color: transparent;
  border-left-color: var(--bg-color);
  right: auto;
  left: -10px;
}

.timeline-date {
  font-weight: bold;
  color: var(--link-color);
  margin-bottom: 5px;
  font-size: 0.9em;
}

.timeline-title {
  font-size: 1.2em;
  font-weight: bold;
  margin-bottom: 5px;
  color: var(--text-color);
}

.timeline-subtitle {
  color: var(--text-muted);
  font-style: italic;
  margin-bottom: 10px;
}

.timeline-description {
  color: var(--text-color);
  line-height: 1.6;
}

.timeline-tags {
  margin-top: 10px;
}

.timeline-tag {
  display: inline-block;
  background: var(--link-color);
  color: white;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.8em;
  margin: 2px;
}

@media (max-width: 768px) {
  .timeline::after {
    left: 31px;
  }
  
  .timeline-item {
    width: 100%;
    padding-left: 70px;
    padding-right: 25px;
  }
  
  .timeline-item:nth-child(even) {
    left: 0;
  }
  
  .timeline-item::after {
    left: 15px;
    right: auto;
  }
  
  .timeline-item:nth-child(even)::after {
    left: 15px;
  }
  
  .timeline-content::after {
    display: none;
  }
}
</style>

<div class="timeline-container">
  <div class="timeline">
    
    <!-- Current Role -->
    <div class="timeline-item">
      <div class="timeline-content">
        <div class="timeline-date">June 2023 - Present</div>
        <div class="timeline-title">Security Engineer</div>
        <div class="timeline-subtitle">Zoho Corporation Pvt. Ltd, India</div>
        <div class="timeline-description">
          Working as a Security Engineer at Zoho Corporation, focusing on application security, 
          vulnerability assessment, and implementing security best practices for enterprise applications.
        </div>
        <div class="timeline-tags">
          <span class="timeline-tag">Security</span>
          <span class="timeline-tag">Application Security</span>
          <span class="timeline-tag">Vulnerability Assessment</span>
        </div>
      </div>
    </div>

    <!-- Education -->
    <div class="timeline-item">
      <div class="timeline-content">
        <div class="timeline-date">March 2019 - 2023</div>
        <div class="timeline-title">Bachelor of Engineering in Electronics and Communication Engineering</div>
        <div class="timeline-subtitle">PSNACET</div>
        <div class="timeline-description">
          Completed Bachelor of Engineering degree in Electronics and Communication Engineering from PSNACET.
        </div>
        <div class="timeline-tags">
          <span class="timeline-tag">Electronics</span>
          <span class="timeline-tag">Communication Engineering</span>
          <span class="timeline-tag">Engineering</span>
        </div>
      </div>
    </div>

  </div>
</div>