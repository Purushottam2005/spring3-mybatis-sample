# spring �g�����U�N�V�����Ǘ�
spring3�ŃA�m�e�[�V�������g�����g�����U�N�V�����Ǘ��ɂ��Ă�������܂Ƃ߂��̂ŋ��L���܂��B
�܂��Aspring�g�����U�N�V�������g�������b�g���ȒP�ɁB

# �����b�g
spring�g�����U�N�V�������g�����ƂŃR�[�h�����ɂ������肵�܂��B���ɃA�m�e�[�V�������g�������@�������߁B�R�[�h�����邾���Ńg�����U�N�V�����Ǘ�����Ă��邩������܂��B
spring�g�����U�N�V������aop���g�p���܂��B

# �R�[�h�Ńg�����U�N�V�����Ǘ�����ƁH
���������Ńg�����U�N�V�����Ǘ�����R�[�h���������ꍇ�Adao�w�ɃR�l�N�V������n���K�v����������A�g�����U�N�V�����J�n�I����commit,rollback�Ƃ������\�[�X���U�݂��邽��
���ɃR�[�h�������Ȃ�B

# �g�����U�N�V�����̋��E
�R���g���[������T�[�r�X�N���X���Ăяo���ꂽ��g�����U�N�V�����J�n�B �T�[�r�X�N���X���I�����ăR���g���[���ɖ߂�Ƃ����g�����U�N�V�����I���B

# �g�����U�N�V�����}�l�[�W���Őݒ�ł���l
* �`�������F�f�t�H���g��PROPAGATION_REQUIRED
* �Ɨ������x���F�f�t�H���g��ISOLATION_READ_COMMITTED
* �^�C���A�E�g�l�F�f�t�H���g��-1(�^�C���A�E�g���Ȃ�)
* �ǂݎ���p�L���F�f�t�H���g��false
* ���[���o�b�N�Ώۗ�O�F�f�t�H���g�͎��s����O�����[���o�b�N�Ώ�
* �R�~�b�g�Ώۗ�O�F�f�t�H���g�͌�����O���R�~�b�g�Ώ�

`TransactionDefinition`�Q��

# �g�p����ƃg�����U�N�V�����}�l�[�W���[�̎���

# �g�����U�N�V�����̋L�q���@
�A�m�e�[�V������xml�L�q���ǂ��Ƃ���Ă���B�\�[�X�R�[�h�ׂ������͊�{NG�Bspring�ł̓A�m�e�[�V�����������߂��Ă���B

## �Ȃ��A�m�e�[�V�������H
�A�m�e�[�V�����ŋL�q����ƃ\�[�X�R�[�h�����邾���Ńg�����U�N�V�����Ώۂ��ۂ��������邵�A�g�����U�N�V�����}�l�[�W���[�̐ݒ���e��������A�g���[�X���₷���B

---

# �A�m�e�[�V�����L�q���@
    @Transactional
    public class ServiceImpl {
    	public void insert(Hoge hoge) {
    		//	insert����
    	}
    	
    	public void update(Hoge hoge) {
    		//	update����
    	}
    	
    	public void search(String id) {
    		//	update����
    	}
    }

��L�� ServiceImpl�̂��ׂẴ��\�b�h���g�����U�N�V�������������B�܂����\�b�h�Ńg�����U�N�V��������ύX�������ꍇ�̓��\�b�h��@Transaction�����㏑������.

    @Transactional
    public class ServiceImpl {
    	@Transactional(timeout=120)
    	public void insert(Hoge hoge) {
    		//	insert����
    	}
    	
    	public void update(Hoge hoge) {
    		//	update����
    	}
    	
    	@Transactional(reandOnly=true)
    	public void search(String id) {
    		//	update����
    	}
    }

## @Transactional(readOnly=true)
�������\�b�h�ɂ�@transactional(readOnly=true)��ݒ肵���ق���RM�Ȃǂ�read-only���œK�����ď������邽�߃p�t�H�[�}���X���o��B

# xml�̐ݒ�
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	
	<tx:annotation-driven />
	