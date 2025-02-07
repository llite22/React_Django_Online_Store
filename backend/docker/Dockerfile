FROM python:3.12-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
ENV PYTHONPATH=/app

# Обновляем и устанавливаем зависимости через apt
RUN apt update \
    && apt install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    curl \
    && apt autoremove -y \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Копируем manage.py и основную папку проекта
COPY store/manage.py .
COPY store/store/ ./store/

RUN python manage.py collectstatic --noinput && \
    chmod -R 755 /app/static

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]