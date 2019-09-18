## flask 后端token验证

#####1.生成token
    def generate_token(key, expire=3600):
        ts_str = str(time.time() + expire)
        ts_byte = ts_str.encode("utf-8")
        sha1_tshexstr = hmac.new(key.encode("utf-8"), ts_byte, 'sha1').hexdigest()
        token = ts_str + ':' + sha1_tshexstr
        b64_token = base64.urlsafe_b64encode(token.encode("utf-8"))
        return b64_token.decode("utf-8")

#####2.验证token,用户id和token
    def certify_token(key, token):
        if key == 'null':
            return False
        token_str = base64.urlsafe_b64decode(token).decode('utf-8')
        token_list = token_str.split(':')
        if len(token_list) != 2:
            return False
        ts_str = token_list[0]
        if float(ts_str) < time.time():
            # token expired
            return False
        known_sha1_tsstr = token_list[1]
        sha1 = hmac.new(key.encode("utf-8"), ts_str.encode('utf-8'), 'sha1')
        calc_sha1_tsstr = sha1.hexdigest()
        if calc_sha1_tsstr != known_sha1_tsstr:
            # token certification failed
            return False
            # token certification success
        return True
        
#####3.用户登录mysql模型
    from passlib.apps import custom_app_context as pwd_context
    from app import db
    

    class User(db.Model):
        __table_name__ = 'user'
        user_id = db.Column(db.INT, primary_key=True)
        user_name = db.Column(db.String(255))
        user_password = db.Column(db.String(255))
    
        def __init__(self, **kwargs):
            self.user_id = kwargs['id_u']
            self.user_name = kwargs['user_name']
            self.user_password = kwargs['user_password']
    
        def hash_password(self, password):
            self.user_password = pwd_context.encrypt(password)
    
        def verify_password(self, password):
            return pwd_context.verify(password, self.user_password)
        
#####4.登录模板
    def login():
        user_name = request.form.get('user_name')
        password = request.form.get('password')
        msg = User.query.filter(User.user_name == user_name).first()
        if msg
            if msg.verify_password(password):
                token = generate_token(str(msg.user_id))
                return jsonify({'status': 200})
        ...