document.addEventListener('DOMContentLoaded', () => {
  const headerNav = document.querySelector('.main-nav');
  const menuToggle = document.querySelector('.menu-toggle');

  if (menuToggle && headerNav) {
    menuToggle.addEventListener('click', () => {
      const isOpen = headerNav.classList.toggle('is-open');
      menuToggle.setAttribute('aria-expanded', String(isOpen));
    });

    headerNav.querySelectorAll('a').forEach((link) => {
      link.addEventListener('click', () => {
        headerNav.classList.remove('is-open');
        menuToggle.setAttribute('aria-expanded', 'false');
      });
    });
  }

  const revealItems = document.querySelectorAll('.reveal');
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          observer.unobserve(entry.target);
        }
      });
    },
    { threshold: 0.15 }
  );

  revealItems.forEach((item) => observer.observe(item));

  const serviceButtons = document.querySelectorAll('.service-btn');
  serviceButtons.forEach((button) => {
    button.addEventListener('click', (event) => {
      const href = button.getAttribute('href');
      if (!href) return;
      if (window.innerWidth <= 767) {
        return true;
      }
      window.open(href, '_blank', 'noopener,noreferrer');
      event.preventDefault();
    });
  });

  const serviceForm = document.getElementById('serviceForm');
  if (serviceForm) {
    serviceForm.addEventListener('submit', (event) => {
      event.preventDefault();

      const formData = new FormData(serviceForm);
      const name = (formData.get('name') || '').toString().trim();
      const phone = (formData.get('phone') || '').toString().trim();
      const service = (formData.get('service') || '').toString().trim();
      const area = (formData.get('area') || '').toString().trim();
      const details = (formData.get('details') || '').toString().trim();

      const message = [
        'مرحبًا نجوم المملكة،',
        'أرغب في طلب خدمة.',
        '',
        `الاسم: ${name}`,
        `رقم الجوال: ${phone}`,
        `الخدمة: ${service}`,
        `الحي: ${area}`,
        `وصف المشكلة: ${details}`
      ].join('\n');

      const whatsappUrl = `https://wa.me/966532336991?text=${encodeURIComponent(message)}`;
      window.open(whatsappUrl, '_blank', 'noopener,noreferrer');
      serviceForm.reset();
    });
  }
});
